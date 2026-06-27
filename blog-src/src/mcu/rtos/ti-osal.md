# TI OSAL API (操作系统抽象层 API)   



## 版本历史 (Version History)

| Version (版本) | Description (描述)                                           | Date (日期) |
| -------------- | ------------------------------------------------------------ | ----------- |
| 1.0            | Initial ZigBee v1.0 release. (初始ZigBee v1.0发布。)         | 04/08/2005  |
| 1.1            | Added note on NV initialization to NV Memory API introduction. (在NV Memory API简介中增加了关于NV初始化的说明。) | 07/22/2005  |
| 1.2            | Modified discussion of the Task Management API. (修改了任务管理API的讨论。) | 08/25/2005  |
| 1.3            | Changed logo on title page, changed copyright on page footer. (更改了标题页的徽标，更改了页脚版权信息。) | 02/27/2006  |
| 1.4            | Modified power management API. (修改了电源管理API。)         | 11/27/2006  |
| 1.5            | Deprecated osal_self() and osalTaskAdd() (废弃了osal_self()和osalTaskAdd()) | 12/18/2007  |
| 1.6            | Added OSAL_Clock functions. (添加了OSAL_Clock函数。)         | 02/21/2008  |
| 1.7            | Updated byte to uint8 and added Misc section. (将byte更新为uint8，并添加了Misc（杂项）章节。) | 04/01/2009  |
| 1.8            | Updated NV item ID table and changed ZSUCCESS to SUCCESS. (更新了NV条目ID表，并将ZSUCCESS改为SUCCESS。) | 04/09/2009  |
| 1.9            | Added a chapter for Simple NV memory system. (添加了简单NV存储系统章节。) | 08/14/2009  |
| 1.10           | Added the osal_msg_find() and osal_start_reload_timer() API. (添加了osal_msg_find()和osal_start_reload_timer() API。) | 11/09/2009  |
| 1.11           | Added osal_nv_delete(), osal_nv_len, osal_run_system(), osal_self() (添加了osal_nv_delete(), osal_nv_len, osal_run_system(), osal_self()) | 06/04/2011  |
| 1.12           | Updated osal_start_timerEx() (更新了osal_start_timerEx())    | 09/19/2011  |

---







## 1. Introduction (简介)

### 1.1 Purpose (目的)

> The purpose of this document is to define the OS Abstraction Layer (OSAL) API. This API allows the software components of a TI stack product, such as Z-Stack™, RemoTI™, and BLE, to be written independently of the specifics of the operating system, kernel or tasking environment (including control loops or connect-to-interrupt systems).

**中文：** 本文档的目的是定义操作系统抽象层 (OSAL) API。该API允许TI协议栈产品（如Z-Stack™、RemoTI™和BLE）的软件组件独立于操作系统、内核或任务环境（包括控制循环或连接中断系统）的具体细节进行编写。



### 1.2 Scope (范围)

> This document enumerates all the function calls provided by the OSAL. The function calls are specified in sufficient detail to allow a programmer to implement them.

**中文：** 本文档列举了OSAL提供的所有函数调用。这些函数调用的规格说明足够详细，以便程序员能够实现它们。



### 1.3 Acronyms (缩略语)

| Acronym (缩略语) | Full Name (全称)                                             |
| ---------------- | ------------------------------------------------------------ |
| API              | Application Programming Interface (应用程序编程接口)         |
| BLE              | Bluetooth Low Energy (低功耗蓝牙)                            |
| NV               | Non-Volatile (非易失性)                                      |
| OSAL             | Operating System (OS) Abstraction Layer (操作系统抽象层)     |
| RF4CE            | RF for Consumer Electronics (消费电子射频4CE)                |
| RemoTI           | Texas Instruments RF4CE protocol stack (德州仪器RF4CE协议栈) |
| Z-Stack          | Texas Instruments ZigBee protocol stack (德州仪器ZigBee协议栈) |

---

## 2. API Overview (API概览)

### 2.1 Overview (概览)

> The OS abstraction layer is used to shield the TI stack software components from the specifics of the processing environment. It provides the following functionality in a manner that is independent of the processing environment.
> 1. Task registration, initialization, starting
> 2. Message exchange between tasks
> 3. Task synchronization
> 4. Interrupt handling
> 5. Timers
> 6. Memory allocation

**中文：** 操作系统抽象层用于将TI协议栈软件组件与处理环境的具体细节隔离开来。它以下述独立于处理环境的方式提供以下功能：

1. 任务注册、初始化、启动
2. 任务间消息交换
3. 任务同步
4. 中断处理
5. 定时器
6. 内存分配

---

## 3. Message Management API (消息管理API)

### 3.1 Introduction (简介)

> The message management API provides a mechanism for exchanging messages between tasks or processing elements with distinct processing environments (for example, interrupt service routines or functions called within a control loop). The functions in this API enable a task to allocate and de-allocate message buffers, send command messages to another task and receive reply messages.

**中文：** 消息管理API提供了一种机制，用于在任务或具有不同处理环境的处理元素（例如，中断服务例程或在控制循环中调用的函数）之间交换消息。该API中的函数允许任务分配和释放消息缓冲区、向另一个任务发送命令消息以及接收回复消息。

### 3.2 osal_msg_allocate( )

#### 3.2.1 Description (描述)

> This function is called by a task to allocate a message buffer, the task/function will then fill in the message and call `osal_msg_send()` to send the message to another task. If the buffer cannot be allocated, `msg_ptr` will be set to NULL.
> **NOTE:** Do not confuse this function with `osal_mem_alloc()`, this function is used to allocate a buffer to send messages between tasks [using `osal_msg_send()`]. Use `osal_mem_alloc()` to allocate blocks of memory.

**中文：** 此函数由任务调用以分配消息缓冲区，然后任务/函数将填充消息并调用 `osal_msg_send()` 将消息发送给另一个任务。如果无法分配缓冲区，`msg_ptr` 将被设置为 NULL。  
**注意：** 请勿将此函数与 `osal_mem_alloc()` 混淆，此函数用于分配缓冲区以便在任务之间发送消息 [使用 `osal_msg_send()`]。使用 `osal_mem_alloc()` 来分配内存块。

#### 3.2.2 Prototype (原型)

```c
uint8 *osal_msg_allocate( uint16 len )
```

#### 3.2.3 Parameter Details (参数详情)

- `len` : the length of the message. (消息的长度。)

#### 3.2.4 Return (返回值)

> The return value is a pointer to the buffer allocated for the message. A NULL return indicates the message allocation operation failed.

**中文：** 返回值是指向为消息分配的缓冲区的指针。返回 NULL 表示消息分配操作失败。

---

### 3.3 osal_msg_deallocate( )

#### 3.3.1 Description (描述)

> This function is used to de-allocate a message buffer. This function is called by a task (or processing element) after it has finished processing a received message.

**中文：** 此函数用于释放消息缓冲区。任务（或处理元素）在处理完接收到的消息后调用此函数。

#### 3.3.2 Prototype (原型)

```c
uint8 osal_msg_deallocate( uint8 *msg_ptr )
```

#### 3.3.3 Parameter Details (参数详情)

- `msg_ptr` : a pointer to the message buffer that needs to be de-allocated. (指向需要释放的消息缓冲区的指针。)

#### 3.3.4 Return (返回值)

> Return value indicates the result of the operation.

**中文：** 返回值指示操作的结果。

| RETURN VALUE (返回值) | DESCRIPTION (描述)                       |
| --------------------- | ---------------------------------------- |
| SUCCESS               | De-allocation Successful (释放成功)      |
| INVALID_MSG_POINTER   | Invalid message pointer (无效的消息指针) |
| MSG_BUFFER_NOT_AVAIL  | Buffer is queued (缓冲区被排队)          |

---

### 3.4 osal_msg_send( )

#### 3.4.1 Description (描述)

> The `osal_msg_send` function is called by a task to send a command or data message to another task or processing element. The `destination_task` identifier field must refer to a valid system task. The `osal_msg_send()` function will also set the `SYS_EVENT_MSG` event in the destination tasks event list.

**中文：** `osal_msg_send` 函数由任务调用，用于向另一个任务或处理元素发送命令或数据消息。`destination_task` 标识符字段必须引用一个有效的系统任务。`osal_msg_send()` 函数还将在目标任务的事件列表中设置 `SYS_EVENT_MSG` 事件。

#### 3.4.2 Prototype (原型)

```c
uint8 osal_msg_send(uint8 destination_task, uint8 *msg_ptr )
```

#### 3.4.3 Parameter Details (参数详情)

- `destination_task` : the ID of the task to receive the message. (接收消息的任务的ID。)
- `msg_ptr` : a pointer to the buffer containing the message. `msg_ptr` must be a pointer to a valid message buffer allocated via `osal_msg_allocate()`. (指向包含消息的缓冲区的指针。`msg_ptr` 必须是通过 `osal_msg_allocate()` 分配的有效消息缓冲区的指针。)

#### 3.4.4 Return (返回值)

> Return value is a 1-byte field indicating the result of the operation.

**中文：** 返回值是一个1字节字段，指示操作的结果。

| RETURN VALUE (返回值) | DESCRIPTION (描述)                            |
| --------------------- | --------------------------------------------- |
| SUCCESS               | Message sent successfully (消息发送成功)      |
| INVALID_MSG_POINTER   | Invalid Message Pointer (无效的消息指针)      |
| INVALID_TASK          | Destination_task is not valid (目标_task无效) |

---

### 3.5 osal_msg_receive( )

#### 3.5.1 Description (描述)

> This function is called by a task to retrieve a received command message. The calling task must de-allocate the message buffer after processing the message using the `osal_msg_deallocate()` call.

**中文：** 此函数由任务调用以检索收到的命令消息。调用任务在处理完消息后必须使用 `osal_msg_deallocate()` 调用来释放消息缓冲区。

#### 3.5.2 Prototype (原型)

```c
uint8 *osal_msg_receive(uint8 task_id )
```

#### 3.5.3 Parameter Details (参数详情)

- `task_id` : the identifier of the calling task (to which the message was destined). (调用任务的标识符（消息的目的地）。)

#### 3.5.4 Return (返回值)

> Return value is a pointer to a buffer containing the message or NULL if there is no received message.

**中文：** 返回值是指向包含消息的缓冲区的指针，如果没有接收到的消息则为 NULL。

---

### 3.6 osal_msg_find( )

#### 3.6.1 Description (描述)

> This function searches for an existing OSAL message matching the `task_id` and event parameters.

**中文：** 此函数搜索与 `task_id` 和事件参数匹配的现有 OSAL 消息。

#### 3.6.2 Prototype (原型)

```c
osal_event_hdr_t *osal_msg_find(uint8 task_id, uint8 event)
```

#### 3.6.3 Parameter Details (参数详情)

- `task_id` : the identifier that the enqueued OSAL message must match. (已排队的 OSAL 消息必须匹配的标识符。)
- `event` : the OSAL event id that the enqueued OSAL message must match. (已排队的 OSAL 消息必须匹配的 OSAL 事件 ID。)

#### 3.6.4 Return (返回值)

> Return value is a pointer to the matching OSAL message on success or NULL on failure.

**中文：** 返回值是指向匹配的 OSAL 消息的指针（成功时），或 NULL（失败时）。

---

## 4. Task Synchronization API (任务同步API)

### 4.1 Introduction (简介)

> This API enables a task to wait for events to happen and return control while waiting. The functions in this API can be used to set events for a task and notify the task once any event is set.

**中文：** 此 API 允许任务等待事件发生，并在等待时返回控制权。该 API 中的函数可用于为任务设置事件，并在任何事件被设置后通知任务。

### 4.2 osal_set_event( )

#### 4.2.1 Description (描述)

> This function is called to set the event flags for a task.

**中文：** 调用此函数为任务设置事件标志。

#### 4.2.2 Prototype (原型)

```c
uint8 osal_set_event(uint8 task_id, uint16 event_flag )
```

#### 4.2.3 Parameter Details (参数详情)

- `task_id` : the identifier of the task for which the event is to be set. (要为其设置事件的任务的标识符。)
- `event_flag` : a 2-byte bitmap with each bit specifying an event. There is only one system event (`SYS_EVENT_MSG`), the rest of the events/bits are defined by the receiving task. (一个2字节的位图，每一位指定一个事件。只有一个系统事件 (`SYS_EVENT_MSG`)，其余的事件/位由接收任务定义。)

#### 4.2.4 Return (返回值)

> Return value indicates the result of the operation.

**中文：** 返回值指示操作的结果。

| RETURN VALUE (返回值) | DESCRIPTION (描述)        |
| --------------------- | ------------------------- |
| SUCCESS               | Success (成功)            |
| INVALID_TASK          | Invalid Task (无效的任务) |

---

## 5. Timer Management API (定时器管理API)

### 5.1 Introduction (简介)

> This API enables the use of timers by internal (TI stack) tasks as well as external (Application level) tasks. The API provides functions to start and stop a timer. The timers can be set in increments of 1 millisecond.

**中文：** 此 API 允许内部（TI 协议栈）任务以及外部（应用层）任务使用定时器。该 API 提供启动和停止定时器的函数。定时器可以以1毫秒为增量进行设置。

### 5.2 osal_start_timerEx( )

#### 5.2.1 Description (描述)

> This function is called to start a timer. When the timer expires, the given event bit will be set. The event will be set for the task specified by `taskID`. This timer is a one shot timer, meaning that when the timer expires it isn’t reloaded.

**中文：** 调用此函数以启动定时器。当定时器到期时，给定的event位将被设置。该事件将为 `taskID` 指定的任务设置。此定时器是单次定时器，意味着当定时器到期时，它不会重新加载。

#### 5.2.2 Prototype (原型)

```c
uint8 osal_start_timerEx( uint8 taskID, uint16 event_id,
                          uint32 timeout_value );
```

#### 5.2.3 Parameter Details (参数详情)

- `taskID` : the task ID of the task that is to get the event when the timer expires. (定时器到期时将获得事件的任务的 ID。)
- `event_id` : a user defined event bit. When the timer expires, the calling task will be notified (event). (用户定义的事件位。当定时器到期时，调用任务将被通知（事件）。)
- `timeout_value` : the amount of time (in milliseconds) before the timer event is set. (定时器事件被设置之前的时间量（以毫秒为单位）。)

#### 5.2.4 Return (返回值)

> Return value indicates the result of the operation.

**中文：** 返回值指示操作的结果。

| RETURN VALUE (返回值) | DESCRIPTION (描述)                         |
| --------------------- | ------------------------------------------ |
| SUCCESS               | Timer Start Successful (定时器启动成功)    |
| NO_TIMER_AVAILABLE    | Unable to start the timer (无法启动定时器) |

---

### 5.3 osal_start_reload_timer( )

#### 5.3.1 Description (描述)

> Call this function to start a timer that, when it expires, will set an event bit and reload the timeout value automatically. The event will be set for the task specified by `taskID`.

**中文：** 调用此函数以启动一个定时器，当它到期时，将设置一个event位并自动重新加载超时值。该事件将为 `taskID` 指定的任务设置。

#### 5.3.2 Prototype (原型)

```c
uint8 osal_start_reload_timer( uint8 taskID, uint16 event_id,
                               uint32 timeout_value );
```

#### 5.3.3 Parameter Details (参数详情)

- `taskID` : the task ID of the task that is to get the event when the timer expires. (定时器到期时将获得事件的任务的 ID。)
- `event_id` : a user defined event bit. When the timer expires, the calling task will be notified (event). (用户定义的事件位。当定时器到期时，调用任务将被通知（事件）。)
- `timeout_value` : the amount of time (in milliseconds) before the timer event is set. This value is reloaded into the timer when the timer expires. (定时器事件设置前的时间量（毫秒）。当定时器到期时，此值会重新加载到定时器中。)

#### 5.3.4 Return (返回值)

> Return value indicates the result of the operation.

**中文：** 返回值指示操作的结果。

| RETURN VALUE (返回值) | DESCRIPTION (描述)                         |
| --------------------- | ------------------------------------------ |
| SUCCESS               | Timer Start Successful (定时器启动成功)    |
| NO_TIMER_AVAILABLE    | Unable to start the timer (无法启动定时器) |

---

### 5.4 osal_stop_timerEx( )

#### 5.4.1 Description (描述)

> This function is called to stop a timer that has already been started. If successful, the function will cancel the timer and prevent the event associated with the timer.

**中文：** 调用此函数以停止已启动的定时器。如果成功，该函数将取消定时器并阻止与该定时器关联的事件。

#### 5.4.2 Prototype (原型)

```c
uint8 osal_stop_timerEx( uint8 task_id, uint16 event_id );
```

#### 5.4.3 Parameter Details (参数详情)

- `task_id` : the task for which to stop the timer. (要为其停止定时器的任务。)
- `event_id` : the identifier of the timer that is to be stopped. (要停止的定时器的标识符。)

#### 5.4.4 Return (返回值)

> Return value indicates the result of the operation.

**中文：** 返回值指示操作的结果。

| RETURN VALUE (返回值) | DESCRIPTION (描述)                          |
| --------------------- | ------------------------------------------- |
| SUCCESS               | Timer Stopped Successfully (定时器停止成功) |
| INVALID_EVENT_ID      | Invalid Event (无效的事件)                  |

---

### 5.5 osal_GetSystemClock( )

#### 5.5.1 Description (描述)

> This function is called to read the system clock.

**中文：** 调用此函数以读取系统时钟。

#### 5.5.2 Prototype (原型)

```c
uint32 osal_GetSystemClock( void );
```

#### 5.5.3 Parameter Details (参数详情)

> None.

**中文：** 无。

#### 5.5.4 Return (返回值)

> The system clock in milliseconds.

**中文：** 以毫秒为单位的系统时钟。

---

## 6. Interrupt Management API (中断管理API)

### 6.1 Introduction (简介)

> This API enables a task to interface with external interrupts. The functions in the API allow a task to associate a specific service routine with each interrupt. The interrupts can be enabled or disabled. Inside the service routine, events may be set for other tasks.

**中文：** 此 API 允许任务与外部中断进行接口。该 API 中的函数允许任务为每个中断关联一个特定的服务例程。中断可以被使能或禁止。在服务例程内部，可以为其他任务设置事件。

### 6.2 osal_int_enable( )

#### 6.2.1 Description (描述)

> This function is called to enable an interrupt. Once enabled, occurrence of the interrupt causes the service routine associated with that interrupt to be called.

**中文：** 调用此函数以使能一个中断。一旦使能，发生中断时将调用与该中断关联的服务例程。

#### 6.2.2 Prototype (原型)

```c
uint8 osal_int_enable( uint8 interrupt_id )
```

#### 6.2.3 Parameter Details (参数详情)

- `interrupt_id` : identifies the interrupt to be enabled. (标识要使能的中断。)

#### 6.2.4 Return (返回值)

> Return value indicates the result of the operation.

**中文：** 返回值指示操作的结果。

| RETURN VALUE (返回值) | DESCRIPTION (描述)                            |
| --------------------- | --------------------------------------------- |
| SUCCESS               | Interrupt Enabled Successfully (中断使能成功) |
| INVALID_INTERRUPT_ID  | Invalid Interrupt (无效的中断)                |

---

### 6.3 osal_int_disable( )

#### 6.3.1 Description (描述)

> This function is called to disable an interrupt. When a disabled interrupt occurs, the service routine associated with that interrupt is not called.

**中文：** 调用此函数以禁止一个中断。当一个被禁止的中断发生时，不会调用与该中断关联的服务例程。

#### 6.3.2 Prototype (原型)

```c
uint8 osal_int_disable( uint8 interrupt_id )
```

#### 6.3.3 Parameter Details (参数详情)

- `interrupt_id` : identifies the interrupt to be disabled. (标识要禁止的中断。)

#### 6.3.4 Return (返回值)

> Return value indicates the result of the operation.

**中文：** 返回值指示操作的结果。

| RETURN VALUE (返回值) | DESCRIPTION (描述)                             |
| --------------------- | ---------------------------------------------- |
| SUCCESS               | Interrupt Disabled Successfully (中断禁止成功) |
| INVALID_INTERRUPT_ID  | Invalid Interrupt (无效的中断)                 |

---

## 7. Task Management API (任务管理API)

### 7.1 Introduction (简介)

> This API is used to add and manage tasks in the OSAL system. Each task is made up of an initialization function and an event processing function. OSAL calls `osalInitTasks()` [application supplied] to initialize the tasks and OSAL uses a task table (`const pTaskEventHandlerFn tasksArr[]`) to call the event processor for each task (also application supplied).

**中文：** 此 API 用于在 OSAL 系统中添加和管理任务。每个任务由一个初始化函数和一个事件处理函数组成。OSAL 调用 `osalInitTasks()` [由应用程序提供] 来初始化任务，并使用任务表 (`const pTaskEventHandlerFn tasksArr[]`) 来调用每个任务的事件处理器（也由应用程序提供）。

> Example of a task table implementation: (任务表示例实现：)

```c
const pTaskEventHandlerFn tasksArr[] =
{
    macEventLoop,
    nwk_event_loop,
    Hal_ProcessEvent,
    MT_ProcessEvent,
    APS_event_loop,
    ZDApp_event_loop,
};
const uint8 tasksCnt = sizeof( tasksArr ) / sizeof( tasksArr[0] );
```

> Example of an `osalInitTasks()` implementation: (`osalInitTasks()` 实现示例：)

```c
void osalInitTasks( void )
{
    uint8 taskID = 0;
    tasksEvents = (uint16 *)osal_mem_alloc( sizeof( uint16 ) * tasksCnt);
    osal_memset( tasksEvents, 0, (sizeof( uint16 ) * tasksCnt));
    macTaskInit( taskID++ );
    nwk_init( taskID++ );
    Hal_Init( taskID++ );
    MT_TaskInit( taskID++ );
    APS_Init( taskID++ );
    ZDApp_Init( taskID++ );
}
```

### 7.2 osal_init_system( )

#### 7.2.1 Description (描述)

> This function initializes the OSAL system. The function must be called at startup prior to using any other OSAL function.

**中文：** 此函数初始化 OSAL 系统。在使用任何其他 OSAL 函数之前，必须在启动时调用此函数。

#### 7.2.2 Prototype (原型)

```c
uint8 osal_init_system( void )
```

#### 7.2.3 Parameter Details (参数详情)

> None.

**中文：** 无。

#### 7.2.4 Return (返回值)

> Return value indicates the result of the operation.

**中文：** 返回值指示操作的结果。

| RETURN VALUE (返回值) | DESCRIPTION (描述) |
| --------------------- | ------------------ |
| SUCCESS               | Success (成功)     |

---

### 7.3 osal_start_system( )

#### 7.3.1 Description (描述)

> This function is the main loop function of the task system, repeatedly calling `osal_run_system()`, from an infinite loop. This function never returns. When using a different scheduler, it should call `osal_run_system()` directly and not use this function.

**中文：** 此函数是任务系统的主循环函数，从一个无限循环中反复调用 `osal_run_system()`。此函数永不返回。当使用不同的调度器时，应直接调用 `osal_run_system()` 而不使用此函数。

#### 7.3.2 Prototype (原型)

```c
void osal_start_system( void )
```

#### 7.3.3 Parameter Details (参数详情)

> None.

**中文：** 无。

#### 7.3.4 Return (返回值)

> None.

**中文：** 无。

---

### 7.4 osal_run_system( )

#### 7.4.1 Description (描述)

> This function will make one pass through the OSAL taskEvents table and call the task_event_processor function for the first task that is found with at least one event pending. After an event is serviced, any remaining events will be returned back to the main loop for next time around. If there are no pending events (for all tasks), this function puts the processor into a Sleep mode.

**中文：** 此函数将遍历一次 OSAL 的 taskEvents 表，并为找到的第一个至少有一个待处理事件的任务调用 task_event_processor 函数。在事件被服务后，任何剩余的事件将返回给主循环以供下次处理。如果没有待处理事件（针对所有任务），此函数将使处理器进入睡眠模式。

#### 7.4.2 Prototype (原型)

```c
void osal_run_system( void )
```

#### 7.4.3 Parameter Details (参数详情)

> None.

**中文：** 无。

#### 7.4.4 Return (返回值)

> None.

**中文：** 无。

---

### 7.5 osal_self( )

#### 7.5.1 Description (描述)

> This function returns the task ID of the currently active OSAL task, corresponding to the index of the task in the OSAL task table. This function can be used by an application to determine the OSAL task ID that it is running under. If called when no OSAL task is active, this function returns the value `TASK_NO_TASK`.

**中文：** 此函数返回当前活动 OSAL 任务的 ID，对应于该任务在 OSAL 任务表中的索引。应用程序可以使用此函数来确定其运行时所处的 OSAL 任务 ID。如果调用时没有活动的 OSAL 任务，此函数将返回值 `TASK_NO_TASK`。

#### 7.5.2 Prototype (原型)

```c
uint8 osal_self( void )
```

#### 7.5.3 Parameter Details (参数详情)

> None.

**中文：** 无。

#### 7.5.4 Return (返回值)

> OSAL task ID of the currently active task.

**中文：** 当前活动任务的 OSAL 任务 ID。

| RETURN VALUE (返回值) | DESCRIPTION (描述)                            |
| --------------------- | --------------------------------------------- |
| 0x00 – 0xFE           | ID of active OSAL task (活动 OSAL 任务的 ID)  |
| 0xFF (TASK_NO_TASK)   | No OSAL task is active (没有活动的 OSAL 任务) |

---

## 8. Memory Management API (内存管理API)

### 8.1 Introduction (简介)

> This API represents a simple memory allocation system. These functions allow dynamic memory allocation.

**中文：** 此 API 代表一个简单的内存分配系统。这些函数允许动态内存分配。

### 8.2 osal_mem_alloc( )

#### 8.2.1 Description (描述)

> This function is a simple memory allocation function that returns a pointer to a buffer (if successful).

**中文：** 此函数是一个简单的内存分配函数，如果成功则返回指向缓冲区的指针。

#### 8.2.2 Prototype (原型)

```c
void *osal_mem_alloc( uint16 size );
```

#### 8.2.3 Parameter Details (参数详情)

- `size` : the number of bytes wanted in the buffer. (缓冲区中需要的字节数。)

#### 8.2.4 Return (返回值)

> A void pointer (which should be cast to the intended buffer type) to the newly allocated buffer. A NULL pointer is returned if there is not enough memory to allocate.

**中文：** 一个指向新分配缓冲区的空指针（应转换为预期的缓冲区类型）。如果没有足够的内存分配，则返回 NULL 指针。

---

### 8.3 osal_mem_free( )

#### 8.3.1 Description (描述)

> This function frees the allocated memory to be used again. This only works if the memory had already been allocated with `osal_mem_alloc()`.

**中文：** 此函数释放已分配的内存以供再次使用。这仅在内存已使用 `osal_mem_alloc()` 分配时才有效。

#### 8.3.2 Prototype (原型)

```c
void osal_mem_free( void *ptr );
```

#### 8.3.3 Parameter Details (参数详情)

- `ptr` : pointer to the buffer to be “freed”. Buffer must have been previously allocated with `osal_mem_alloc()`. (指向要“释放”的缓冲区的指针。缓冲区必须先前已使用 `osal_mem_alloc()` 分配。)

#### 8.3.4 Return (返回值)

> None.

**中文：** 无。

---

## 9. Power Management API (电源管理API)

### 9.1 Introduction (简介)

> This section describes the OSAL power management system. The system provides a way for the applications/tasks to notify OSAL when it is safe to turn off the receiver and external hardware, and put the processor in to sleep.
> There are two functions to control the power management. The first, `osal_pwrmgr_device()`, is called to set the device level mode (power save or no power savings). Then, there is the task power state, each task can hold off the power manager from conserving power by calling `osal_pwrmgr_task_state( PWRMGR_HOLD )`. If a task “Holds” the power manager, it will need to call `osal_pwrmgr_task_state( PWRMGR_CONSERVE )` to allow the power manager to continue in power conserve mode.
> By default, when the task is initialized, each task’s power state is set to `PWRMGR_CONSERVE`, so if a task does not want to hold off power conservation (no change) it doesn’t need to call `osal_pwrmgr_task_state()`.
> In addition, by default, a battery-operated device will be in `PWRMGR_ALWAYS_ON` state until it joins the network, then it will change its state to `PWRMGR_BATTERY`. This means that if the device cannot find a device to join it will not enter a power save state. If you want to change this behavior, add `osal_pwrmgr_device( PWRMGR_BATTERY )` in your application’s task initialization function or when your application stops/pauses the joining process.
> The power manager will look at the device mode and the collective power state of all the tasks before going in to power conserve state.

**中文：** 本节描述 OSAL 电源管理系统。该系统为应用程序/任务提供了一种方式，以便在可以安全关闭接收器和外部硬件并使处理器进入睡眠状态时通知 OSAL。
有两个函数用于控制电源管理。第一个是 `osal_pwrmgr_device()`，用于设置设备级模式（省电或无省电）。然后是任务电源状态，每个任务可以通过调用 `osal_pwrmgr_task_state( PWRMGR_HOLD )` 来阻止电源管理器省电。如果任务“保持”了电源管理器，它将需要调用 `osal_pwrmgr_task_state( PWRMGR_CONSERVE )` 以允许电源管理器继续省电模式。
默认情况下，当任务初始化时，每个任务的电源状态都设置为 `PWRMGR_CONSERVE`，因此如果任务不想阻止省电（无更改），则无需调用 `osal_pwrmgr_task_state()`。
此外，默认情况下，电池供电的设备在加入网络之前将处于 `PWRMGR_ALWAYS_ON` 状态，然后将其状态更改为 `PWRMGR_BATTERY`。这意味着如果设备找不到要加入的设备，它将不会进入省电状态。如果您想更改此行为，请在应用程序的任务初始化函数中或当应用程序停止/暂停加入过程时添加 `osal_pwrmgr_device( PWRMGR_BATTERY )`。
在进入省电状态之前，电源管理器将查看设备模式和所有任务的集体电源状态。

### 9.2 osal_pwrmgr_init( )

#### 9.2.1 Description (描述)

> This function will initialize variables used by the power management system. **IMPORTANT:** Do not call this function, it is already called by `osal_init_system()`.

**中文：** 此函数将初始化电源管理系统使用的变量。**重要：** 不要调用此函数，它已由 `osal_init_system()` 调用。

#### 9.2.2 Prototype (原型)

```c
void osal_pwrmgr_init( void );
```

#### 9.2.3 Parameter Details (参数详情)

> None.

**中文：** 无。

#### 9.2.4 Return (返回值)

> None.

**中文：** 无。

---

### 9.3 osal_pwrmgr_powerconserve( )

#### 9.3.1 Description (描述)

> This function is called to go into power down mode. **IMPORTANT:** Do not call this function, it is already called in the OSAL main loop [`osal_start_system()`].

**中文：** 调用此函数以进入掉电模式。**重要：** 不要调用此函数，它已在 OSAL 主循环 [`osal_start_system()`] 中被调用。

#### 9.3.2 Prototype (原型)

```c
void osal_pwrmgr_powerconserve( void );
```

#### 9.3.3 Parameter Details (参数详情)

> None.

**中文：** 无。

#### 9.3.4 Return (返回值)

> None.

**中：** 无。

---

### 9.4 osal_pwrmgr_device( )

#### 9.4.1 Description (描述)

> This function is called on power-up or whenever the power requirements change (ex. Battery backed coordinator). This function sets the overall ON/OFF State of the device’s power manager. This function should be called from a central controlling entity (like ZDO).

**中文：** 此函数在上电时或电源需求发生变化时（例如，电池供电的协调器）调用。此函数设置设备电源管理器的总体开/关状态。此函数应由中央控制实体（如 ZDO）调用。

#### 9.4.2 Prototype (原型)

```c
void osal_pwrmgr_device( uint8 pwrmgr_device );
```

#### 9.4.3 Parameter Details (参数详情)

- `Pwrmgr_device` : changes or sets the power savings mode. (更改或设置省电模式。)

| Type (类型)      | Description (描述)                                           |
| ---------------- | ------------------------------------------------------------ |
| PWRMGR_ALWAYS_ON | With this selection, there is no power savings and the device is most likely on mains power. (选择此项不省电，设备很可能使用市电。) |
| PWRMGR_BATTERY   | Turns power savings on. (开启省电。)                         |

#### 9.4.4 Return (返回值)

> None.

**中文：** 无。

---

### 9.5 osal_pwrmgr_task_state( )

#### 9.5.1 Description (描述)

> This function is called by each task to state whether or not this task wants to conserve power. The task will call this function to vote whether it wants the OSAL to conserve power or it wants to hold off on the power savings. By default, when a task is created, its own power state is set to conserve. If the task always wants to converse power, it does not need to call this function at all.

**中文：** 此函数由每个任务调用，以声明此任务是否希望省电。任务将调用此函数来表决它希望 OSAL 省电还是希望暂缓省电。默认情况下，当任务创建时，其自身的电源状态设置为省电。如果任务始终希望省电，则完全不需要调用此函数。

#### 9.5.2 Prototype (原型)

```c
uint8 osal_pwrmgr_task_state(uint8 task_id, uint8 state );
```

#### 9.5.3 Parameter Details (参数详情)

- `state` : changes a task’s power state. (更改任务的电源状态。)

| Type (类型)     | Description (描述)                                           |
| --------------- | ------------------------------------------------------------ |
| PWRMGR_CONSERVE | Turns power savings on, all tasks have to agree. This is the default state when a task is initialized. (开启省电，所有任务必须同意。这是任务初始化时的默认状态。) |
| PWRMGR_HOLD     | Turns power savings off. (关闭省电。)                        |

#### 9.5.4 Return (返回值)

> Return value indicates the result of the operation.

**中文：** 返回值指示操作的结果。

| RETURN VALUE (返回值) | DESCRIPTION (描述)        |
| --------------------- | ------------------------- |
| SUCCESS               | Success (成功)            |
| INVALID_TASK          | Invalid Task (无效的任务) |

---

## 10. Non-Volatile Memory API (非易失性存储器API)

### 10.1 Introduction (简介)

> This section describes the OSAL Non-Volatile (NV) memory system. The system provides a way for applications to store information into the device’s memory persistently. It is also used by the stack for persistent storage of certain items required by the ZigBee specification. The NV functions are designed to read and write user-defined items consisting of arbitrary data types such as structures or arrays. The user can read or write an entire item or an element of the item by setting the appropriate offset and length. The API is independent of the NV storage medium and can be implemented for flash or EEPROM.
> Each NV item has a unique ID. There is a specific ID value range for applications while some ID values are reserved or used by the stack or platform. If your application creates its own NV item, it must select an ID from the Application value range. See the table below.

**中文：** 本节描述 OSAL 非易失性 (NV) 存储系统。该系统为应用程序提供了一种将信息持久存储到设备内存中的方法。它也由协议栈用于持久存储 ZigBee 规范所需的某些项目。NV 函数旨在读取和写入由任意数据类型（如结构体或数组）组成的用户定义项目。用户可以通过设置适当的偏移量和长度来读取或写入整个项目或项目的一个元素。该 API 独立于 NV 存储介质，可以为闪存或 EEPROM 实现。
每个 NV 项目都有一个唯一的 ID。应用程序有一个特定的 ID 值范围，而某些 ID 值由协议栈或平台保留或使用。如果您的应用程序创建自己的 NV 项目，则必须从应用程序值范围中选择一个 ID。请参见下表。

| VALUE (值)      | USER (用户)                                                  |
| --------------- | ------------------------------------------------------------ |
| 0x0000          | Reserved (保留)                                              |
| 0x0001 – 0x0020 | OSAL                                                         |
| 0x0021 – 0x0040 | NWK                                                          |
| 0x0041 – 0x0060 | APS                                                          |
| 0x0061 – 0x0080 | Security (安全)                                              |
| 0x0081 – 0x00B0 | ZDO                                                          |
| 0x00B1 – 0x00E0 | Commissioning SAS (调试 SAS)                                 |
| 0x00E1 – 0x0100 | Reserved (保留)                                              |
| 0x0101 – 0x01FF | Trust Center Link Keys (信任中心链接密钥)                    |
| 0x0201 – 0x0300 | ZigBee-Pro: APS Links Keys / ZigBee-RF4CE: network layer (ZigBee-Pro: APS 链接密钥 / ZigBee-RF4CE: 网络层) |
| 0x0301 – 0x0400 | ZigBee-Pro: Master Keys / ZigBee-RF4CE: app framework (ZigBee-Pro: 主密钥 / ZigBee-RF4CE: 应用程序框架) |
| 0x0401 – 0x0FFF | Application (应用程序)                                       |
| 0x1000 -0xFFFF  | Reserved (保留)                                              |

> There are some important considerations when using this API:
> 1. These are blocking function calls and an operation may take several milliseconds to complete. This is especially true for NV write operations. In addition, interrupts may be disabled for several milliseconds. It is best to execute these functions at times when they do not conflict with other timing-critical operations. For example, a good time to write NV items would be when the receiver is turned off.
> 2. Try to perform NV writes infrequently. It takes time and power; also most flash devices have a limited number of erase cycles.
> 3. If the structure of one or more NV items changes, especially when upgrading from one version of a TI stack software to another, it is necessary to erase and re-initialize the NV memory. Otherwise, read and write operations on NV items that changed will fail or produce erroneous results.

**中文：** 使用此 API 时有一些重要的注意事项：
1. 这些都是阻塞性函数调用，操作可能需要几毫秒才能完成。对于 NV 写操作尤其如此。此外，中断可能会被禁用几毫秒。最好在不与其他时间关键操作冲突时执行这些函数。例如，写入 NV 项目的好时机是当接收器关闭时。
2. 尽量不要频繁执行 NV 写操作。这需要时间和功耗；此外，大多数闪存设备的擦除周期次数有限。
3. 如果一个或多个 NV 项目的结构发生变化，尤其是在从一个版本的 TI 协议栈软件升级到另一个版本时，必须擦除并重新初始化 NV 存储器。否则，对已更改的 NV 项目的读写操作将失败或产生错误结果。

---

### 10.2 osal_nv_item_init( )

#### 10.2.1 Description (描述)

> Initialize an item in NV. This function checks for the presence of an item in NV. If it does not exist, it is created and initialized with the data passed to the function, if any.
> This function must be called for each item before calling `osal_nv_read()` or `osal_nv_write()`.

**中文：** 在 NV 中初始化一个项目。此函数检查 NV 中是否存在某个项目。如果不存在，则创建它，并（如果有）使用传递给函数的数据进行初始化。
在调用 `osal_nv_read()` 或 `osal_nv_write()` 之前，必须为每个项目调用此函数。

#### 10.2.2 Prototype (原型)

```c
uint8 osal_nv_item_init( uint16 id, uint16 len, void *buf );
```

#### 10.2.3 Parameter Details (参数详情)

- `id` : User-defined item ID. (用户定义的项目 ID。)
- `len` : Item length in bytes. (项目长度，以字节为单位。)
- `*buf` : Pointer to item initialization data. If no initialization data, set to NULL. (指向项目初始化数据的指针。如果没有初始化数据，设置为 NULL。)

#### 10.2.4 Return (返回值)

> Return value indicates the result of the operation.

**中文：** 返回值指示操作的结果。

| RETURN VALUE (返回值) | DESCRIPTION (描述)                                  |
| --------------------- | --------------------------------------------------- |
| SUCCESS               | Success (成功)                                      |
| NV_ITEM_UNINIT        | Success but item did not exist (成功，但项目不存在) |
| NV_OPER_FAILED        | Operation failed (操作失败)                         |

---

### 10.3 osal_nv_read( )

#### 10.3.1 Description (描述)

> Read data from NV. This function can be used to read an entire item from NV or an element of an item by indexing into the item with an offset. Read data is copied into `*buf`.

**中文：** 从 NV 读取数据。此函数可用于从 NV 读取整个项目，或通过使用偏移量索引到项目中来读取项目的一个元素。读取的数据被复制到 `*buf` 中。

#### 10.3.2 Prototype (原型)

```c
uint8 osal_nv_read( uint16 id, uint16 offset, uint16 len, void *buf );
```

#### 10.3.3 Parameter Details (参数详情)

- `id` : User-defined item ID. (用户定义的项目 ID。)
- `offset` : Memory offset into item in bytes. (项目内的内存偏移量，以字节为单位。)
- `len` : Item length in bytes. (项目长度，以字节为单位。)
- `*buf` : Data is read into this buffer. (数据被读入此缓冲区。)

#### 10.3.4 Return (返回值)

> Return value indicates the result of the operation.

**中文：** 返回值指示操作的结果。

| RETURN VALUE (返回值) | DESCRIPTION (描述)          |
| --------------------- | --------------------------- |
| SUCCESS               | Success (成功)              |
| NV_OPER_FAILED        | Operation failed (操作失败) |

---

### 10.4 osal_nv_write( )

#### 10.4.1 Description (描述)

> Write data to NV. This function can be used to write an entire item to NV or an element of an item by indexing into the item with an offset.

**中文：** 向 NV 写入数据。此函数可用于将整个项目写入 NV，或通过使用偏移量索引到项目中来写入项目的一个元素。

#### 10.4.2 Prototype (原型)

```c
uint8 osal_nv_write( uint16 id, uint16 offset, uint16 len, void *buf );
```

#### 10.4.3 Parameter Details (参数详情)

- `id` : User-defined item ID. (用户定义的项目 ID。)
- `offset` : Memory offset into item in bytes. (项目内的内存偏移量，以字节为单位。)
- `len` : Item length in bytes. (项目长度，以字节为单位。)
- `*buf` : Data to write. (要写入的数据。)

#### 10.4.4 Return (返回值)

> Return value indicates the result of the operation.

**中文：** 返回值指示操作的结果。

| RETURN VALUE (返回值) | DESCRIPTION (描述)                     |
| --------------------- | -------------------------------------- |
| SUCCESS               | Success (成功)                         |
| NV_ITEM_UNINIT        | Item is not initialized (项目未初始化) |
| NV_OPER_FAILED        | Operation failed (操作失败)            |

---

### 10.5 osal_nv_delete( )

#### 10.5.1 Description (描述)

> Delete an item from NV. This function checks for the presence of the item in NV. If the item exists and its length matches the length provided in the function call, the item will be removed from NV.

**中文：** 从 NV 中删除一个项目。此函数检查 NV 中是否存在该项目。如果项目存在并且其长度与函数调用中提供的长度匹配，则该项目将从 NV 中删除。

#### 10.5.2 Prototype (原型)

```c
uint8 osal_nv_delete( uint16 id, uint16 len );
```

#### 10.5.3 Parameter Details (参数详情)

- `id` : User-defined item ID. (用户定义的项目 ID。)
- `len` : Item length in bytes. (项目长度，以字节为单位。)

#### 10.5.4 Return (返回值)

> Return value indicates the result of the operation.

**中文：** 返回值指示操作的结果。

| RETURN VALUE (返回值) | DESCRIPTION (描述)                            |
| --------------------- | --------------------------------------------- |
| SUCCESS               | Success (成功)                                |
| NV_ITEM_UNINIT        | Item is not initialized (项目未初始化)        |
| NV_BAD_ITEM_LEN       | Incorrect length parameter (不正确的长度参数) |
| NV_OPER_FAILED        | Operation failed (操作失败)                   |

---

### 10.6 osal_nv_item_len( )

#### 10.6.1 Description (描述)

> Get length of an item in NV. This function returns the length of an NV item, if found, otherwise zero.

**中文：** 获取 NV 中项目的长度。此函数返回 NV 项目的长度（如果找到），否则返回零。

#### 10.6.2 Prototype (原型)

```c
uint16 osal_nv_item_len( uint16 id );
```

#### 10.6.3 Parameter Details (参数详情)

- `id` : User-defined item ID. (用户定义的项目 ID。)

#### 10.6.4 Return (返回值)

> Return value indicates the result of the operation.

**中文：** 返回值指示操作的结果。

| RETURN VALUE (返回值) | DESCRIPTION (描述)                 |
| --------------------- | ---------------------------------- |
| 0                     | NV item not found (未找到 NV 项目) |
| 1 - N                 | Length of NV item (NV 项目的长度)  |

---

### 10.7 osal_offsetof( )

#### 10.7.1 Description (描述)

> This macro calculates the memory offset in bytes of an element within a structure. It is useful for calculating the offset parameter used by NV API functions.

**中文：** 此宏计算结构体中某个元素的内存偏移量（以字节为单位）。这对于计算 NV API 函数使用的偏移量参数非常有用。

#### 10.7.2 Prototype (原型)

```c
osal_offsetof(type, member)
```

#### 10.7.3 Parameter Details (参数详情)

- `type` : Structure type. (结构体类型。)
- `member` : Structure member. (结构体成员。)

---

## 11. Simple Non-Volatile Memory API (简单非易失性存储器API)

### 11.1 Introduction (简介)

> This section describes the OSAL Simple Non-Volatile memory system. Like the OSAL NV memory system, the Simple NV memory system provides a way for applications to store information into the device’s memory persistently. On the other hand, unlike the OSAL NV memory system, the Simple NV memory system provides much simpler API to drive the application code size and the stack code size down as well as the code size of the OSAL Simple NV system implementation. The user can read or write an entire item, but it cannot partially read or write the item.
> Just like NV memory system, each NV item has a unique ID. There is a specific ID value range for applications while some ID values are reserved or used by the stack or platform. If your application creates its own NV items, it must select an ID from the Application value range. See the table below.
> Note that the table might not apply to a certain custom stack build.

**中文：** 本节描述 OSAL 简单非易失性存储系统。与 OSAL NV 存储系统一样，简单 NV 存储系统为应用程序提供了一种将信息持久存储到设备内存中的方法。另一方面，与 OSAL NV 存储系统不同，简单 NV 存储系统提供了更简单的 API，以减少应用程序代码大小、协议栈代码大小以及 OSAL 简单 NV 系统实现的代码大小。用户可以读取或写入整个项目，但不能部分读取或写入项目。
就像 NV 存储系统一样，每个 NV 项目都有一个唯一的 ID。应用程序有一个特定的 ID 值范围，而某些 ID 值由协议栈或平台保留或使用。如果您的应用程序创建自己的 NV 项目，则必须从应用程序值范围中选择一个 ID。请参见下表。
请注意，该表可能不适用于某些自定义协议栈构建。

| VALUE (值)  | USER (用户)                                                  |
| ----------- | ------------------------------------------------------------ |
| 0x00        | Reserved (保留)                                              |
| 0x01 – 0x6F | Reserved for ZigBee RF4CE network layer (为 ZigBee RF4CE 网络层保留) |
| 0x70 – 0x7F | Reserved for ZigBee RF4CE application framework (RTI) (为 ZigBee RF4CE 应用程序框架 (RTI) 保留) |
| 0x80 – 0xFE | Application (应用程序)                                       |
| 0xFF        | Reserved (保留)                                              |

> There are some important considerations when using this API:
> 1. These are blocking function calls and an operation may take several hundred milliseconds to complete. This is especially true for NV write operations. In addition, interrupts may be disabled for several milliseconds. It is best to execute these functions at times when they do not conflict with other timing-critical operations. For example, a good time to write NV items would be when the receiver is turned off.
> 2. Furthermore, the functions must not be called from an interrupt service routine, unless otherwise specified by a separate application note or release note of an implementation.
> 3. Try to perform NV writes infrequently. It takes time and power; also most flash devices have a limited number of erase cycles.
> 4. If the structure of one or more NV items changes, especially when upgrading from one version of a TI stack software to another, it is necessary to erase and re-initialize the NV memory. Otherwise, read and write operations on NV items that changed will fail or produce erroneous results.

**中文：** 使用此 API 时有一些重要的注意事项：
1. 这些都是阻塞性函数调用，操作可能需要几百毫秒才能完成。对于 NV 写操作尤其如此。此外，中断可能会被禁用几毫秒。最好在不与其他时间关键操作冲突时执行这些函数。例如，写入 NV 项目的好时机是当接收器关闭时。
2. 此外，不得从中断服务例程调用这些函数，除非某个实现的单独应用笔记或版本说明另有规定。
3. 尽量不要频繁执行 NV 写操作。这需要时间和功耗；此外，大多数闪存设备的擦除周期次数有限。
4. 如果一个或多个 NV 项目的结构发生变化，尤其是在从一个版本的 TI 协议栈软件升级到另一个版本时，必须擦除并重新初始化 NV 存储器。否则，对已更改的 NV 项目的读写操作将失败或产生错误结果。

---

### 11.2 osal_snv_read( )

#### 11.2.1 Description (描述)

> Read data from NV. This function can be used to read an entire item from NV. Read data is copied into `*pBuf`.

**中文：** 从 NV 读取数据。此函数可用于从 NV 读取整个项目。读取的数据被复制到 `*pBuf` 中。

#### 11.2.2 Prototype (原型)

```c
uint8 osal_snv_read( osalSnvId_t id, osalSnvLen_t len, void *pBuf );
```

#### 11.2.3 Parameter Details (参数详情)

- `id` : User-defined item ID. (用户定义的项目 ID。)
- `len` : Item length in bytes. (项目长度，以字节为单位。)
- `*pBuf` : Data is read into this buffer. (数据被读入此缓冲区。)

#### 11.2.4 Return (返回值)

> Return value indicates the result of the operation. Note that an attempt to read an item, which was never written before, would result into the `NV_OPER_FAILED` return code.

**中文：** 返回值指示操作的结果。请注意，尝试读取一个之前从未写入过的项目将导致 `NV_OPER_FAILED` 返回码。

| RETURN VALUE (返回值) | DESCRIPTION (描述)          |
| --------------------- | --------------------------- |
| SUCCESS               | Success (成功)              |
| NV_OPER_FAILED        | Operation failed (操作失败) |

---

### 11.3 osal_snv_write( )

#### 11.3.1 Description (描述)

> Write data to NV. This function can be used to write an entire item to NV.

**中文：** 向 NV 写入数据。此函数可用于将整个项目写入 NV。

#### 11.3.2 Prototype (原型)

```c
uint8 osal_snv_write( osalSnvId_t id, osalSnvLen_t len, void *pBuf );
```

#### 11.3.3 Parameter Details (参数详情)

- `id` : User-defined item ID. (用户定义的项目 ID。)
- `len` : Item length in bytes. (项目长度，以字节为单位。)
- `*pBuf` : Data to write. (要写入的数据。)

#### 11.3.4 Return (返回值)

> Return value indicates the result of the operation. Note that it is allowed to write an item that was never before initialized into the NV system by other means.

**中文：** 返回值指示操作的结果。请注意，允许写入一个之前从未通过其他方式初始化到 NV 系统中的项目。

| RETURN VALUE (返回值) | DESCRIPTION (描述)          |
| --------------------- | --------------------------- |
| SUCCESS               | Success (成功)              |
| NV_OPER_FAILED        | Operation failed (操作失败) |

---

## 12. OSAL Clock System (OSAL时钟系统)

### 12.1 Introduction (简介)

> This section describes the OSAL Clock system. The system provides a way to keep date and time for a device. This system will keep the number of seconds since 0 hrs, 0 minutes, 0 seconds on 1 January 2000 UTC. The following two data types/structures are used in this system (defined in `OSAL_Clock.h`):

**中文：** 本节描述 OSAL 时钟系统。该系统为设备提供了一种保持日期和时间的方法。该系统将保持自 UTC 时间 2000 年 1 月 1 日 0 时 0 分 0 秒以来的秒数。该系统使用以下两种数据类型/结构（在 `OSAL_Clock.h` 中定义）：

```c
// number of seconds since 0 hrs, 0 minutes, 0 seconds, on the
// 1st of January 2000 UTC
typedef uint32 UTCTime;

// To be used with
typedef struct
{
    uint8 seconds;  // 0-59
    uint8 minutes;  // 0-59
    uint8 hour;     // 0-23
    uint8 day;      // 0-30
    uint8 month;    // 0-11
    uint16 year;    // 2000+
} UTCTimeStruct;
```

> You must enable the `OSAL_CLOCK` compiler flag to use this feature. In addition, this feature does not maintain time for sleeping devices.

**中文：** 必须启用 `OSAL_CLOCK` 编译器标志才能使用此功能。此外，此功能不为睡眠设备维护时间。

---

### 12.2 osalTimeUpdate( )

#### 12.2.1 Description (描述)

> Called from `osal_run_system()` to update the time. This function reads the number of MAC 320usec ticks to maintain the OSAL Clock time. Do not call this function anywhere else.

**中文：** 从 `osal_run_system()` 调用以更新时间。此函数读取 MAC 320 微秒滴答数以维护 OSAL 时钟时间。请勿在其他任何地方调用此函数。

#### 12.2.2 Prototype (原型)

```c
void osalTimeUpdate( void );
```

#### 12.2.3 Parameter Details (参数详情)

> None.

**中文：** 无。

#### 12.2.4 Return (返回值)

> None.

**中文：** 无。

---

### 12.3 osal_setClock( )

#### 12.3.1 Description (描述)

> Call this function to initialize the device’s time.

**中文：** 调用此函数以初始化设备的时间。

#### 12.3.2 Prototype (原型)

```c
void osal_setClock( UTCTime newTime );
```

#### 12.3.3 Parameter Details (参数详情)

- `newTime` : new time in seconds since 0 hrs, 0 minutes, 0 seconds, on 1 January 2000 UTC. (自 UTC 时间 2000 年 1 月 1 日 0 时 0 分 0 秒以来的新时间，以秒为单位。)

#### 12.3.4 Return (返回值)

> None.

**中文：** 无。

---

### 12.4 osal_getClock( )

#### 12.4.1 Description (描述)

> Call this function to retrieve the device’s current time.

**中文：** 调用此函数以检索设备的当前时间。

#### 12.4.2 Prototype (原型)

```c
UTCTime osal_getClock( void );
```

#### 12.4.3 Parameter Details (参数详情)

> None.

**中文：** 无。

#### 12.4.4 Return (返回值)

> Current time, in seconds since zero hrs, 0 minutes, 0 seconds, on 1 January 2000 UTC.

**中文：** 当前时间，自 UTC 时间 2000 年 1 月 1 日 0 时 0 分 0 秒以来的秒数。

---

### 12.5 osal_ConvertUTCTime( )

#### 12.5.1 Description (描述)

> Call this function to convert `UTCTime` to `UTCTimeStruct`.

**中文：** 调用此函数将 `UTCTime` 转换为 `UTCTimeStruct`。

#### 12.5.2 Prototype (原型)

```c
void osal_ConvertUTCTime( UTCTimeStruct * tm, UTCTime secTime );
```

#### 12.5.3 Parameter Details (参数详情)

- `secTime` : time in seconds since 0 hrs, 0 minutes, 0 seconds, on 1 January 2000 UTC. (自 UTC 时间 2000 年 1 月 1 日 0 时 0 分 0 秒以来的时间，以秒为单位。)
- `tm` : pointer to time structure. (指向时间结构体的指针。)

#### 12.5.4 Return (返回值)

> None.

**中文：** 无。

---

### 12.6 osal_ConvertUTCSecs( )

#### 12.6.1 Description (描述)

> Call this function to convert `UTCTimeStruct` to `UTCTime`.

**中文：** 调用此函数将 `UTCTimeStruct` 转换为 `UTCTime`。

#### 12.6.2 Prototype (原型)

```c
UTCTime osal_ConvertUTCSecs( UTCTimeStruct * tm );
```

#### 12.6.3 Parameter Details (参数详情)

- `tm` : pointer to time structure. (指向时间结构体的指针。)

#### 12.6.4 Return (返回值)

> Converted time, in seconds since 0 hrs, 0 minutes, 0 seconds, on 1 January 2000 UTC.

**中文：** 转换后的时间，自 UTC 时间 2000 年 1 月 1 日 0 时 0 分 0 秒以来的秒数。

---

## 13. OSAL Misc (OSAL杂项)

### 13.1 Introduction (简介)

> This section describes miscellaneous OSAL functions that do not fit into the previous OSAL categories.

**中文：** 本节描述不属于前面 OSAL 类别的杂项 OSAL 函数。

### 13.2 osal_rand( )

#### 13.2.1 Description (描述)

> This function returns a 16-bit random number.

**中文：** 此函数返回一个 16 位随机数。

#### 13.2.2 Prototype (原型)

```c
uint16 osal_rand( void );
```

#### 13.2.3 Parameter Details (参数详情)

> None.

**中文：** 无。

#### 13.2.4 Return (返回值)

> Returns a random number.

**中文：** 返回一个随机数。

---

### 13.3 osal_memcmp( )

#### 13.3.1 Description (描述)

> Compare to memory sections.

**中文：** 比较两个内存区域。

#### 13.3.2 Prototype (原型)

```c
uint8 osal_memcmp( const void GENERIC *src1, const void GENERIC *src2,
                    unsigned int len );
```

#### 13.3.3 Parameter Details (参数详情)

- `src1` : memory compare location 1. (内存比较位置1。)
- `src2` : memory compare location 2. (内存比较位置2。)
- `len` : length of compare. (比较的长度。)

#### 13.3.4 Return (返回值)

> TRUE - same, FALSE - different.

**中文：** TRUE - 相同，FALSE - 不同。

---

### 13.4 osal_memset( )

#### 13.4.1 Description (描述)

> Sets a buffer to a specific value.

**中文：** 将缓冲区设置为特定值。

#### 13.4.2 Prototype (原型)

```c
void *osal_memset( void *dest, uint8 value, int len );
```

#### 13.4.3 Parameter Details (参数详情)

- `dest` : memory buffer to set “value” to. (要设置为“value”的内存缓冲区。)
- `value` : what to set each byte of “dest”. (要设置“dest”每个字节的值。)
- `len` : length to set “value” in “dest”. (在“dest”中设置“value”的长度。)

#### 13.4.4 Return (返回值)

> Pointer to where in the buffer this function stopped.

**中文：** 指向此函数在缓冲区中停止位置的指针。

---

### 13.5 osal_memcpy( )

#### 13.5.1 Description (描述)

> Copies one buffer to another buffer.

**中文：** 将一个缓冲区复制到另一个缓冲区。

#### 13.5.2 Prototype (原型)

```c
void *osal_memcpy( void *dst, const void GENERIC *src, unsigned int len );
```

#### 13.5.3 Parameter Details (参数详情)

- `dst` : destination buffer. (目标缓冲区。)
- `src` : source buffer. (源缓冲区。)
- `len` : length of copy. (复制的长度。)

#### 13.5.4 Return (返回值)

> Pointer to end of destination buffer.

**中文：** 指向目标缓冲区末尾的指针。

---

**版权所有 © 2005-2011 Texas Instruments, Inc. 保留所有权利。**
