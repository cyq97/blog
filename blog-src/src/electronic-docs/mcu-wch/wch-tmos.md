# TMOS 调度系统使用指南

## 概述

TMOS (Task Management Operating System) 是沁恒 BLE 协议栈内置的一个**轻量级协作式任务调度器**。它不是抢占式 RTOS，不涉及线程、栈切换等复杂概念。核心思想很简单：**每个任务注册一个事件处理函数，系统在 `TMOS_SystemProcess()` 中轮询调度**。

```
main() {
    TMOS_TimerInit(...);          // 1. 初始化定时器
    ...
    TMOS_ProcessEventRegister(handler1);  // 2. 注册任务
    TMOS_ProcessEventRegister(handler2);
    ...
    while(1) {
        TMOS_SystemProcess();     // 3. 主循环：处理定时器 + 派发事件
    }
}
```

## 核心概念

### 时间基准

TMOS 的时间单位是 **625μs**（1 个 system tick）。所有定时参数都以 ticks 为单位。

```c
#define SYSTEM_TIME_MICROSEN  625
#define MS1_TO_SYSTEM_TIME(x)  ((x) * 1000 / SYSTEM_TIME_MICROSEN)  // 毫秒 → ticks
```

换算关系：
- `1 ms` ≈ 1.6 ticks
- `1600` ticks = 1 秒
- `3200` ticks = 2 秒

### 任务 ID (`tmosTaskID`)

`uint8_t` 类型，范围 0~255。通过 `TMOS_ProcessEventRegister()` 获取。

### 事件 (`tmosEvents`)

`uint16_t` 位掩码，每个 bit 代表一个事件。**低 15 位（bit0~14）由用户自定义**，最高位 `SYS_EVENT_MSG` (0x8000) 由系统保留，用于消息传递。

```c
#define SYS_EVENT_MSG  0x8000   // 有消息待处理
#define INVALID_TASK_ID 0xFF    // 非法任务ID
```

一个任务可以同时被多个事件触发：

```c
uint16 events = SBP_START_DEVICE_EVT | SBP_PARAM_UPDATE_EVT;
```

### 任务处理函数

```c
typedef tmosEvents (*pTaskEventHandlerFn)(tmosTaskID taskID, tmosEvents event);
```

返回值是**未处理的事件**。返回 0 表示所有事件都已处理完毕。

```c
uint16 MyTask_ProcessEvent(uint8 task_id, uint16 events)
{
    if (events & MY_EVENT_A)     // 检查事件A
    {
        // 处理...
        return (events ^ MY_EVENT_A);  // 清除已处理的事件
    }

    if (events & SYS_EVENT_MSG)  // 检查消息
    {
        uint8 *msg = tmos_msg_receive(task_id);
        if (msg) {
            // 处理消息...
            tmos_msg_deallocate(msg);  // 释放消息
        }
        return (events ^ SYS_EVENT_MSG);
    }
    return 0;  // 未知事件，全部丢弃
}
```

---

## API 参考

### 注册任务

```c
tmosTaskID TMOS_ProcessEventRegister(pTaskEventHandlerFn eventCb);
```

返回任务 ID，失败返回 `INVALID_TASK_ID` (0xFF)。

示例：
```c
uint8_t MyTaskID;
MyTaskID = TMOS_ProcessEventRegister(MyTask_ProcessEvent);
if (MyTaskID == INVALID_TASK_ID) { /* 错误处理 */ }
```

### 立即触发事件

```c
bStatus_t tmos_set_event(tmosTaskID taskID, tmosEvents event);
```

将事件立即置位，下次 `TMOS_SystemProcess()` 时触发。

**注意**：不能在任务自己的事件处理函数内调用 `tmos_set_event` 给自己设置同一个事件（会导致死循环）。如需重入，用 `tmos_start_task`。

### 延迟触发事件（单次）

```c
BOOL tmos_start_task(tmosTaskID taskID, tmosEvents event, tmosTimer time);
```

在 `time` 个 ticks 后触发指定事件一次。

```c
// 1 秒后触发 MY_EVENT
tmos_start_task(MyTaskID, MY_EVENT, 1600);
// 10ms 后触发
tmos_start_task(MyTaskID, MY_EVENT, MS1_TO_SYSTEM_TIME(10));
```

### 周期触发事件（自动重载）

```c
bStatus_t tmos_start_reload_task(tmosTaskID taskID, tmosEvents event, tmosTimer time);
```

与 `tmos_start_task` 类似，但触发后**自动重新装载**，实现周期性定时。

```c
// 每 500ms 触发一次
tmos_start_reload_task(MyTaskID, MY_PERIODIC_EVT, MS1_TO_SYSTEM_TIME(500));
```

### 停止定时器

```c
bStatus_t tmos_stop_task(tmosTaskID taskID, tmosEvents event);
```

取消尚未触发的定时事件。

### 清除事件

```c
bStatus_t tmos_clear_event(tmosTaskID taskID, tmosEvents event);
```

将已触发但尚未处理的事件清除。**不能在自己的事件处理函数内使用。**

### 查询剩余时间

```c
tmosTimer tmos_get_task_timer(tmosTaskID taskID, tmosEvents event);
```

返回该事件距离到期的剩余 ticks 数。

---

## 消息传递

TMOS 支持任务间发送消息。消息以 `SYS_EVENT_MSG` 方式通知接收任务。

### 发送消息

```c
bStatus_t tmos_msg_send(tmosTaskID taskID, uint8_t *msg_ptr);
```

发送消息后，接收任务的 `SYS_EVENT_MSG` 事件会被置位。

### 分配/释放消息

```c
uint8_t *tmos_msg_allocate(uint16_t len);         // 分配消息缓冲区
bStatus_t tmos_msg_deallocate(uint8_t *msg_ptr);  // 释放消息
```

发送消息前，必须先用 `tmos_msg_allocate` 分配内存。接收方处理完后调用 `tmos_msg_deallocate` 释放。

### 接收消息

```c
uint8_t *tmos_msg_receive(tmosTaskID taskID);
```

在任务的事件处理函数中，收到 `SYS_EVENT_MSG` 时调用。返回消息指针，无消息时返回 `NULL`。

### 完整示例：发送带数据的消息

```c
// 发送方
typedef struct {
    tmos_event_hdr_t hdr;   // 必须第一个字段: {event, status}
    uint16_t value;
} my_msg_t;

void send_msg(uint8_t target_task)
{
    my_msg_t *msg = (my_msg_t *)tmos_msg_allocate(sizeof(my_msg_t));
    if (msg) {
        msg->hdr.event = MY_CUSTOM_MSG;  // 自定义消息类型
        msg->hdr.status = 0;
        msg->value = 42;
        tmos_msg_send(target_task, (uint8_t *)msg);
    }
}

// 接收方
uint16 MyTask_ProcessEvent(uint8 task_id, uint16 events)
{
    if (events & SYS_EVENT_MSG) {
        uint8_t *p = tmos_msg_receive(task_id);
        while (p) {
            my_msg_t *msg = (my_msg_t *)p;
            if (msg->hdr.event == MY_CUSTOM_MSG) {
                // 处理 msg->value
            }
            tmos_msg_deallocate(p);
            p = tmos_msg_receive(task_id);  // 可能有多个消息排队
        }
        return (events ^ SYS_EVENT_MSG);
    }
    return 0;
}
```

---

## 实用工具函数

```c
void   tmos_memcpy(void *dst, const void *src, uint32_t len);
void   tmos_memset(void *dst, uint8_t val, uint32_t len);
BOOL   tmos_memcmp(const void *s1, const void *s2, uint32_t len);
BOOL   tmos_isbufset(uint8_t *buf, uint8_t val, uint32_t len);
uint32_t tmos_strlen(char *str);
uint32_t tmos_rand(void);
```

---

## 常见用法模式

### 模式 1：单次延迟执行

```c
#define LED_OFF_EVT  0x0001

uint16 App_ProcessEvent(uint8 task_id, uint16 events)
{
    if (events & LED_OFF_EVT) {
        LED_Off();
        return (events ^ LED_OFF_EVT);
    }
    return 0;
}

void app_init(void)
{
    AppTaskID = TMOS_ProcessEventRegister(App_ProcessEvent);
    // 按键按下，3 秒后关灯
    tmos_start_task(AppTaskID, LED_OFF_EVT, MS1_TO_SYSTEM_TIME(3000));
}
```

### 模式 2：周期性轮询

```c
#define POLL_EVT  0x0002

uint16 App_ProcessEvent(uint8 task_id, uint16 events)
{
    if (events & POLL_EVT) {
        read_sensor();
        // 自动重复，无需手动续约
        return (events ^ POLL_EVT);
    }
    return 0;
}

void app_init(void)
{
    AppTaskID = TMOS_ProcessEventRegister(App_ProcessEvent);
    tmos_start_reload_task(AppTaskID, POLL_EVT, MS1_TO_SYSTEM_TIME(100));
}
```

### 模式 3：手动续约

```c
uint16 App_ProcessEvent(uint8 task_id, uint16 events)
{
    if (events & SEND_EVT) {
        if (is_ready()) {
            send_data();
            tmos_start_task(task_id, SEND_EVT, SEND_INTERVAL);  // 续约
        }
        // 不 ready 时不再续约，链自然中断
        return (events ^ SEND_EVT);
    }
    return 0;
}
```

### 模式 4：从 BLE 回调延迟发送

TMOS 任务的事件处理函数在**主循环上下文**中执行，而 BLE 消息回调在协议栈内部上下文中执行。如果在回调里直接调用 BLE 发送 API 可能失败，需要通过 TMOS 延迟到主循环执行：

```c
// BLE 回调中（不能直接发数据）
void on_ble_rx(uint16 conn, uint8_t *data, uint16_t len)
{
    // 把数据缓存起来
    tmos_memcpy(echo_buf, data, len);
    echo_len = len;
    echo_conn = conn;
    // 延迟到下一个 TMOS tick
    tmos_start_task(AppTaskID, ECHO_SEND_EVT, 1);
}

// TMOS 任务处理函数（主循环上下文，可以安全调用 BLE API）
uint16 App_ProcessEvent(uint8 task_id, uint16 events)
{
    if (events & ECHO_SEND_EVT) {
        ch_send(echo_conn, echo_buf, echo_len);
        return (events ^ ECHO_SEND_EVT);
    }
    return 0;
}
```

---

## 关键注意事项

1. **不要在中断里调用 TMOS API**。TMOS 不是线程安全的，所有操作必须在主循环上下文。
2. **事件位不要冲突**。每个任务的 bit0~14 可以自由定义，但要确保不同事件的掩码不重叠。
3. **`tmos_set_event` vs `tmos_start_task`**：前者立即触发（当前 tick），后者延迟触发。重入场景用后者。
4. **消息必须释放**：`tmos_msg_receive` 之后必须 `tmos_msg_deallocate`，否则内存泄漏。
5. **`TMOS_SystemProcess()` 驱动一切**：它处理时钟更新、定时器到期、事件派发。如果长时间不调用，所有定时器都会延迟。
6. **定时精度有限**：TMOS 的 RTC 时钟精度取决于配置的时钟源（内部 LSI 约 32kHz，外部 LSE 32.768kHz），不适合微秒级精确定时。

---

## 与 HAL 层的关系

`HAL_Init()` 内部调用了 `TMOS_TimerInit()`，并注册了 HAL 任务。用户不需要手动初始化 TMOS 时钟，只需注册自己的任务即可。
