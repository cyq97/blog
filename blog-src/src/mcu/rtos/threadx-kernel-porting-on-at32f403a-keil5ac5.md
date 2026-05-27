# Threadx 内核移植 AT32F403A Keil5-AC5





## 最简移植

1. Threadx 内核源码目录: `threadx仓库/common`
   - 将此目录下 `src` 目录中的源文件**全部**添加到工程中
   - 将此目录下 `inc` 目录的添加到工程的头文件搜索路径中

2. Threadx Port 目录: `threadx仓库/ports/cortex_m4/keil`
   - 将此目录下 `src` 目录中的源文件**全部**添加到工程中，包括
   - 将此目录下 `inc` 目录的添加到工程的头文件搜索路径中

3. 手动创建一个 `tx_initialize_low_level.c` 文件添加到工程中，文件内容如下

   ```C
   
   #define REG_SCB_SHP(n)              (*(volatile unsigned char*)(0xE000ED18UL + (n)))
   
   #define REG_SYSTICK_CTRL            (*(volatile unsigned int*)(0xE000E010UL))
   #define REG_SYSTICK_LOAD            (*(volatile unsigned int*)(0xE000E014UL))
   #define REG_SYSTICK_VAL             (*(volatile unsigned int*)(0xE000E018UL))
   #define BIT_SYSTICK_CTRL_ENABLE     ((unsigned int)(1 << 0))
   #define BIT_SYSTICK_CTRL_TICKINT    ((unsigned int)(1 << 1))
   #define BIT_SYSTICK_CTRL_CLKSRC     ((unsigned int)(1 << 2))
   
   
   extern unsigned int system_core_clock;
   extern void _tx_timer_interrupt(void);
   
   
   void _tx_initialize_low_level(void)
   {
     // 关闭中断
     {
       //__disable_irq();
       __asm__ volatile ("CPSID i");
     }
   
     // 设置系统异常的优先级
     {
       REG_SCB_SHP( 0) =    0; // MemManage_Handler
       REG_SCB_SHP( 1) =    0; // BusFault_Handler
       REG_SCB_SHP( 2) =    0; // UsageFault_Handler
       REG_SCB_SHP( 3) =    0; // ----
       REG_SCB_SHP( 4) =    0; // ----
       REG_SCB_SHP( 5) =    0; // ----
       REG_SCB_SHP( 6) =    0; // ----
       REG_SCB_SHP( 7) = 0xFF; // SVC_Handler
       REG_SCB_SHP( 8) =    0; // DebugMon_Handler
       REG_SCB_SHP( 9) =    0; // ----
       REG_SCB_SHP(10) = 0xFF; // PendSV_Handler
       REG_SCB_SHP(11) = 0x40; // SysTick_Handler
     }
     
     // 配置DWT
     {
       // TO DO
     }
   
     // 配置Systick
     {
       // 停止Systick
       // 关闭Systick中断
       REG_SYSTICK_CTRL &= ~(BIT_SYSTICK_CTRL_ENABLE | BIT_SYSTICK_CTRL_TICKINT);
       
       // 设置Systick时钟为内核时钟
       REG_SYSTICK_CTRL |=  (BIT_SYSTICK_CTRL_CLKSRC);
     
       // 清除Systick计数器
       REG_SYSTICK_VAL   =  (0x00000000UL);
     
       // 设置Systick重装载值
       const unsigned int rtos_freq = 1000;
       REG_SYSTICK_LOAD  =  (system_core_clock / rtos_freq) - 1;
       
       // 开启Systick中断
       // 启动Systick
       REG_SYSTICK_CTRL |=  (BIT_SYSTICK_CTRL_ENABLE | BIT_SYSTICK_CTRL_TICKINT);
     }
   }
   
   
   void SysTick_Handler()
   {
       _tx_timer_interrupt();
   }
   
   
   ```

   



## 小记

1. ThreadX 内核只使用了 PendSV 和 Sysctick 这两个内核中断
