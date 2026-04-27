# AT24C256C Datasheet Extract
# AT24C256C 数据手册提取

**Source / 来源:** Atmel-AT24C256C-Datasheet.pdf（21 pages）  
**Extracted / 提取日期:** 2026-04-26  
**Scope / 范围:** 全文技术内容（双语，英文原文 + 中文注释）

---

## Table of Contents / 目录

1. [Features & Description / 特性与描述](#1-features--description--特性与描述)
2. [Pin Configurations and Pinouts / 引脚配置与定义](#2-pin-configurations-and-pinouts--引脚配置与定义)
3. [Absolute Maximum Ratings / 绝对最大额定值](#3-absolute-maximum-ratings--绝对最大额定值)
4. [Pin Descriptions / 引脚说明](#4-pin-descriptions--引脚说明)
5. [Memory Organization / 存储器组织](#5-memory-organization--存储器组织)
6. [DC Characteristics / 直流特性](#6-dc-characteristics--直流特性)
7. [AC Characteristics / 交流特性（时序参数）](#7-ac-characteristics--交流特性时序参数)
8. [Device Addressing / 器件寻址](#8-device-addressing--器件寻址)
9. [Write Operations / 写操作](#9-write-operations--写操作)
10. [Read Operations / 读操作](#10-read-operations--读操作)
11. [Block Diagram / 功能框图](#11-block-diagram--功能框图)
12. [Revision History / 版本历史](#12-revision-history--版本历史)

---

## 1. Features & Description / 特性与描述

### Features / 特性

- Low-voltage and standard-voltage operation  
  **低电压与标准电压工作**
  - V_CC = 1.7V to 5.5V
- Internally organized as 32,768 × 8  
  **内部组织为 32,768 × 8（即 256Kbit）**
- 2-wire serial interface  
  **2 线串行接口（I²C）**
- Schmitt Trigger, filtered inputs for noise suppression  
  **施密特触发器输入，带滤波，抑制噪声**
- Bidirectional data transfer protocol  
  **双向数据传输协议**
- 400kHz (1.7V) and 1MHz (2.5V, 2.7V, 5.0V) compatibility  
  **支持 400kHz（1.7V 时）和 1MHz（2.5V/2.7V/5.0V 时）**
- Write Protect pin for hardware protection  
  **写保护引脚（WP），提供硬件写保护**
- 64-byte page write mode  
  **64 字节页写模式**
- Partial page writes allowed  
  **允许部分页写入**
- Self-timed write cycle (5ms max)  
  **自定时写周期，最大 5ms**
- High reliability  
  **高可靠性**
  - Endurance: 1,000,000 write cycles / **耐久性：100 万次写操作**
  - Data retention: 40 years / **数据保持：40 年**
- Lead-free/Halogen-free devices available  
  **提供无铅/无卤素器件**
- Green package options (Pb/Halide-free/RoHS compliant)  
  **绿色封装（无铅/无卤/符合 RoHS）**
- 8-lead JEDEC SOIC, 8-lead TSSOP, 8-pad UDFN, and 8-ball VFBGA packages  
  **封装：8 引脚 SOIC、8 引脚 TSSOP、8 焊盘 UDFN、8 球 VFBGA**
- Die sale options: wafer form, waffle pack, and bumped wafers  
  **裸晶销售选项：晶圆形式、晶粒盘、凸点晶圆**

### Description / 描述

The Atmel® AT24C256C provides 262,144-bits of Serial Electrically Erasable and Programmable Read-Only Memory (EEPROM) organized as 32,768 words of eight bits each. The device's cascading feature allows up to eight devices to share a common 2-wire bus. The device is optimized for use in many industrial and commercial applications where low-power and low-voltage operation are essential. The devices are available in space-saving 8-lead JEDEC SOIC, 8-lead TSSOP, 8-pad UDFN, and 8-ball VFBGA packages. In addition, this device operates from 1.7V to 5.5V.

**AT24C256C 提供 262,144 位串行电可擦写可编程只读存储器（EEPROM），内部组织为 32,768 个 8 位字。器件的级联特性允许多达 8 个器件共享一条公共 2 线总线。该器件专为低功耗、低电压的工业和商业应用而优化。提供节省空间的 8 引脚 SOIC、8 引脚 TSSOP、8 焊盘 UDFN 和 8 球 VFBGA 封装，工作电压范围 1.7V 至 5.5V。**

---

## 2. Pin Configurations and Pinouts / 引脚配置与定义

> 参考 PDF 第 2 页，Table 1-1 Pin Configuration（含各封装引脚图）

### Table 1-1. Pin Configuration / 引脚配置

| Pin Name | Function | 中文说明 |
|----------|----------|----------|
| A0 | Address Input | 地址输入位 0 |
| A1 | Address Input | 地址输入位 1 |
| A2 | Address Input | 地址输入位 2 |
| GND | Ground | 地（参考地） |
| SDA | Serial Data | 串行数据（双向） |
| SCL | Serial Clock Input | 串行时钟输入 |
| WP | Write Protect | 写保护 |
| VCC | Device Power Supply | 器件电源 |

**封装引脚排列（PDF 第 2 页图示）：**
- **8-lead SOIC / 8-lead TSSOP：** A0(1) A1(2) A2(3) GND(4) — SDA(5) SCL(6) WP(7) VCC(8)
- **8-pad UDFN：** 同上，Top View
- **8-ball VFBGA：** Bottom View，VCC(8,1) WP(7,2) SCL(6,3) SDA(5,4) — GND(4) A2(3) A1(2) A0(1)

---

## 3. Absolute Maximum Ratings / 绝对最大额定值

> 参考 PDF 第 2 页，Section 2

| Parameter / 参数 | Rating / 额定值 |
|----------------|----------------|
| Operating Temperature / 工作温度 | −55°C to +125°C |
| Storage Temperature / 存储温度 | −65°C to +150°C |
| Voltage on any pin w.r.t. GND / 任意引脚对地电压 | −1.0V to +7.0V |
| Maximum Operating Voltage / 最大工作电压 | 6.25V |
| DC Output Current / 直流输出电流 | 5.0mA |

> **注意 / Note:** Stresses beyond those listed under "Absolute Maximum Ratings" may cause permanent damage to the device. This is a stress rating only and functional operation at these or any other conditions beyond those indicated in the operational sections is not implied.  
> **超过绝对最大额定值的应力可能导致器件永久损坏。此为应力等级，不代表在这些条件下器件可正常工作。**

---

## 4. Pin Descriptions / 引脚说明

> 参考 PDF 第 3 页，Section 4

**Serial Clock (SCL) / 串行时钟：**  
The SCL input is used to positive-edge clock data into each EEPROM device and negative-edge clock data out of each device.  
**SCL 输入用于在上升沿将数据锁入器件，在下降沿将数据从器件输出。**

**Serial Data (SDA) / 串行数据：**  
The SDA pin is bidirectional for serial data transfer. This pin is open drain driven and may be wire-ORed with any number of other open-drain or open-collector devices.  
**SDA 引脚为双向串行数据传输引脚，开漏驱动，可与多个开漏或集电极开路器件进行线与连接。**

**Device Addresses (A2, A1, A0) / 器件地址：**  
The A2, A1, and A0 pins are device address inputs that are hard wired (directly to GND or to VCC) for compatibility with other Atmel AT24C devices. When the pins are hard wired, as many as eight 256K devices may be addressed on a single bus system. If these pins are left floating, the A2, A1, and A0 pins will be internally pulled down to GND. However, due to capacitive coupling that may appear during customer applications, Atmel recommends always connecting the address pins to a known state. When using a pull-up resistor, Atmel recommends using 10kΩ or less.  
**A2、A1、A0 为器件地址输入引脚，应硬连线至 GND 或 VCC。最多可在同一总线上寻址 8 个 256K 器件。引脚悬空时内部下拉至 GND，但建议始终连接至已知状态，上拉电阻建议 10kΩ 以下。**

**Write Protect (WP) / 写保护：**  
When connected to GND, allows normal write operations. When WP is connected directly to VCC, all write operations to the memory are inhibited. If the pin is left floating, the WP pin will be internally pulled down to GND. Atmel recommends always connecting the WP pin to a known state.  
**WP 接 GND 时允许正常写操作；接 VCC 时禁止所有写操作。悬空时内部下拉至 GND，建议连接至已知状态。**

### Table 4-1. Write Protect / 写保护状态

| WP Pin Status | Part of the Array Protected |
|--------------|----------------------------|
| At VCC（接电源） | Full Array / 全部阵列受保护 |
| At GND（接地） | Normal Read/Write Operations / 正常读写操作 |

---

## 5. Memory Organization / 存储器组织

> 参考 PDF 第 4 页，Section 5

The AT24C256C, 256K Serial EEPROM: The 256K is internally organized as 512 pages of 64-bytes each. Random word addressing requires a 15-bit data word address.  
**AT24C256C 内部组织为 512 页 × 64 字节。随机字地址访问需要 15 位字地址。**

### Table 5-1. Pin Capacitance / 引脚电容

> Applicable over recommended operating range: T_A = 25°C, f = 1.0MHz, V_CC = 1.7V to 5.5V.  
> **测试条件：T_A = 25°C，f = 1.0MHz，V_CC = 1.7V～5.5V**

| Symbol | Parameter / 参数 | Max | Units | Conditions |
|--------|-----------------|-----|-------|------------|
| C_I/O | Input/Output Capacitance (SDA) / SDA 输入输出电容 | 8 | pF | V_I/O = 0V |
| C_IN | Input Capacitance (A0, A1, A2, SCL) / 地址及时钟输入电容 | 6 | pF | V_IN = 0V |

> Note: This parameter is characterized and is not 100% tested. / **注：此参数为特征值，非 100% 测试。**

---

## 6. DC Characteristics / 直流特性

> 参考 PDF 第 4 页，Table 5-2  
> Applicable over recommended operating range: T_AI = −40°C to +85°C, V_CC = 1.7V to 5.5V (unless otherwise noted).  
> **适用范围：T_AI = −40°C～+85°C，V_CC = 1.7V～5.5V（除非另有说明）**

| Symbol | Parameter / 参数 | Test Condition / 测试条件 | Min | Typ | Max | Units |
|--------|-----------------|--------------------------|-----|-----|-----|-------|
| V_CC | Supply Voltage / 电源电压 | — | 1.7 | — | 5.5 | V |
| I_CC1 | Supply Current (Read) / 读工作电流 | V_CC = 5.0V, Read at 400kHz | — | 1.0 | 2.0 | mA |
| I_CC2 | Supply Current (Write) / 写工作电流 | V_CC = 5.0V, Write at 400kHz | — | 2.0 | 3.0 | mA |
| I_SB1 | Standby Current / 待机电流 | V_IN = V_CC or V_SS, V_CC = 1.7V | — | — | 1.0 | μA |
| I_SB1 | Standby Current / 待机电流 | V_IN = V_CC or V_SS, V_CC = 5.0V | — | — | 6.0 | μA |
| I_LI | Input Leakage Current / 输入漏电流 | V_IN = V_CC or V_SS, V_CC = 5.0V | — | 0.10 | 3.0 | μA |
| I_LO | Output Leakage Current / 输出漏电流 | V_OUT = V_CC or V_SS, V_CC = 5.0V | — | 0.05 | 3.0 | μA |
| V_IL | Input Low Level / 输入低电平 | — | −0.6 | — | V_CC × 0.3 | V |
| V_IH | Input High Level / 输入高电平 | — | V_CC × 0.7 | — | V_CC + 0.5 | V |
| V_OL1 | Output Low Level / 输出低电平 | V_CC = 1.7V, I_OL = 0.15mA | — | — | 0.2 | V |
| V_OL2 | Output Low Level / 输出低电平 | V_CC = 3.0V, I_OL = 2.1mA | — | — | 0.4 | V |

> Note 1: V_IL min and V_IH max are reference only and are not tested. / **注 1：V_IL 最小值和 V_IH 最大值仅供参考，不做 100% 测试。**

---

## 7. AC Characteristics / 交流特性（时序参数）

> 参考 PDF 第 5～6 页，Section 6  
> Applicable over recommended operating range: T_A = −40°C to +85°C, V_CC = 1.7V to 5.5V.

### Table 6-1. AC Characteristics — 400kHz Mode / 400kHz 模式时序参数

| Symbol | Parameter / 参数 | 1.7V Min | 1.7V Max | 2.5V–5.5V Min | 2.5V–5.5V Max | Units |
|--------|-----------------|----------|----------|---------------|---------------|-------|
| f_SCL | SCL Clock Frequency / SCL 时钟频率 | — | 400 | — | 1000 | kHz |
| t_LOW | SCL Low Period / SCL 低电平时间 | 1300 | — | 600 | — | ns |
| t_HIGH | SCL High Period / SCL 高电平时间 | 600 | — | 600 | — | ns |
| t_AA | SCL Low to SDA Data Out / SCL 低到 SDA 数据输出 | — | 900 | — | 900 | ns |
| t_BUF | Bus Free Time between STOP and START / STOP 后总线空闲时间 | 1300 | — | 500 | — | ns |
| t_HD:STA | START Hold Time / START 保持时间 | 600 | — | 250 | — | ns |
| t_SU:STA | START Setup Time / START 建立时间 | 600 | — | 250 | — | ns |
| t_HD:DAT | Data Input Hold Time / 数据输入保持时间 | 0 | — | 0 | — | ns |
| t_SU:DAT | Data Input Setup Time / 数据输入建立时间 | 100 | — | 100 | — | ns |
| t_SU:STO | STOP Setup Time / STOP 建立时间 | 600 | — | 250 | — | ns |
| t_R | SDA and SCL Rise Time / SDA 和 SCL 上升时间 | — | 300 | — | 300 | ns |
| t_F | SDA and SCL Fall Time / SDA 和 SCL 下降时间 | — | 300 | — | 300 | ns |
| t_DH | Data Output Hold Time / 数据输出保持时间 | 50 | — | 50 | — | ns |
| t_WR | Write Cycle Time / 写周期时间 | — | 5 | — | 5 | ms |

> 参见 PDF 第 5 页 Figure 6-1（400kHz 模式时序图）及第 6 页 Figure 6-2（输入/输出波形）

### AC Test Conditions / 交流测试条件

> 参考 PDF 第 6 页，Table 6-2

| Parameter / 参数 | Value |
|----------------|-------|
| Input pulse levels / 输入脉冲电平 | 0.1 VCC to 0.9 VCC |
| Input rise and fall times / 输入上升/下降时间 | ≤ 10ns |
| Input and output timing reference levels / 输入输出时序参考电平 | 0.5 VCC |
| Output load / 输出负载 | Current source: I_OL = 3mA (V_CC = 5V), C_L = 100pF |

---

## 8. Device Addressing / 器件寻址

> 参考 PDF 第 7～9 页，Section 7

The AT24C256C device has a 4-bit device type identifier (1010) hardcoded in the most significant four bits of the slave address byte. The remaining three bits (A2, A1, A0) are the device address bits. The least significant bit (R/W) selects the operation: 1 = Read, 0 = Write.  
**AT24C256C 器件地址高 4 位固定为 1010（器件类型标识符），后 3 位为器件地址位（A2、A1、A0），最低位为读写控制位（1 = 读，0 = 写）。**

### Slave Address Byte / 从机地址字节

```
Bit 7  Bit 6  Bit 5  Bit 4  Bit 3  Bit 2  Bit 1  Bit 0
  1      0      1      0     A2     A1     A0    R/W
```

- A2, A1, A0 correspond to the hardware pins. Up to 8 devices on the same bus.  
  **A2、A1、A0 对应硬件引脚，同一总线最多 8 个器件。**
- R/W = 0: Write operation / **写操作**
- R/W = 1: Read operation / **读操作**

### Acknowledge / 应答

After each byte received, the AT24C256C sends an ACK (pulls SDA low during the 9th clock pulse). During read operations, the master must send a NACK (leaves SDA high) followed by a STOP to terminate the read.  
**每接收到一个字节后，AT24C256C 在第 9 个时钟脉冲期间将 SDA 拉低以发送 ACK。读操作结束时，主机发送 NACK（SDA 高）后再发 STOP 条件。**

---

## 9. Write Operations / 写操作

> 参考 PDF 第 9～11 页，Section 8

### 9.1 Byte Write / 字节写

The master initiates a write by sending a START condition, followed by the device address with R/W = 0 (ACK from slave), then two bytes of word address (high byte first), then one data byte, then STOP. The device enters an internally timed write cycle (t_WR max 5ms), during which it does not respond to I²C requests.  
**主机发起写操作：发送 START → 器件地址（R/W=0，从机 ACK）→ 两字节字地址（高字节在前）→ 1 字节数据 → STOP。器件随后进入内部自定时写周期（最大 5ms），期间不响应 I²C 请求。**

> 参见 PDF 第 9 页，Figure 8-1（字节写时序图）

### 9.2 Page Write / 页写

A page write is initiated the same way as a byte write, but instead of sending a STOP after the first data byte, the master sends up to 63 additional bytes. Each data byte increments the internal address counter (lower 6 bits only — page rollover occurs if more than 64 bytes sent).  
**页写初始化方式与字节写相同，但在发送第一个数据字节后主机继续发送最多 63 个额外字节，内部地址计数器（低 6 位）自动递增。超过 64 字节时发生页内回卷。**

> 参见 PDF 第 10 页，Figure 8-2（页写时序图）

### 9.3 Acknowledge Polling / 应答轮询

Since the AT24C256C does not respond while its internal write cycle is in progress, the master can determine when the write cycle is complete by continuously issuing a START followed by the device address. Once the write cycle is complete, the device issues an ACK.  
**在内部写周期未完成时器件不应答，主机可通过持续发送 START + 器件地址来轮询写操作是否完成，器件 ACK 即表示写周期结束。**

> 参见 PDF 第 10 页，Figure 8-3（应答轮询时序图）

### 9.4 Write Protection / 写保护

- WP = GND: Normal write operations allowed / **WP 接地：允许正常写操作**
- WP = VCC: All write operations inhibited / **WP 接电源：禁止所有写操作**

---

## 10. Read Operations / 读操作

> 参考 PDF 第 11～14 页，Section 9

### 10.1 Current Address Read / 当前地址读

The internal address counter maintains the last address accessed during the last read or write operation, incremented by one. The master issues a START, device address with R/W = 1, and the device outputs the data byte at the current address, then master sends NACK and STOP.  
**内部地址计数器保持上次访问地址加 1。主机发送 START → 器件地址（R/W=1）→ 器件输出当前地址数据字节 → 主机发 NACK + STOP。**

> 参见 PDF 第 12 页，Figure 9-1（当前地址读时序图）

### 10.2 Random Read / 随机读

A random read requires a "dummy" write to load the address. The master sends START, device address (R/W=0), two address bytes (ACK from device each), then a repeated START, device address (R/W=1). The device then outputs the data byte, and master sends NACK and STOP.  
**随机读需要先进行"虚写"以加载地址：START → 器件地址（R/W=0）→ 两字节地址 → 重复 START → 器件地址（R/W=1）→ 器件输出数据字节 → 主机 NACK + STOP。**

> 参见 PDF 第 12 页，Figure 9-2（随机读时序图）

### 10.3 Sequential Read / 顺序读

Sequential reads are initiated in the same way as a current address read or random read, but instead of sending NACK after the first byte, the master sends ACK to continue reading. The internal address counter automatically increments with each byte read. The counter rolls over from the last address of the last memory page to the first address of the first page.  
**顺序读的初始化方式与当前地址读或随机读相同，但主机在收到第一字节后发送 ACK 以继续读取。内部地址计数器随每字节自动递增，从最后一页末尾回卷到第一页首地址。**

> 参见 PDF 第 13 页，Figure 9-3（顺序读时序图）

---

## 11. Block Diagram / 功能框图

> 参考 PDF 第 3 页，Section 3，Figure（功能框图）

功能框图主要组成模块（参见 PDF 第 3 页图示）：

| Module / 模块 | Description / 说明 |
|--------------|-------------------|
| Start/Stop Logic | START/STOP 条件检测逻辑 |
| Serial Control Logic | 串行控制逻辑 |
| H.V. Pump/Timing | 高压泵与时序控制（用于 EEPROM 写操作） |
| Device Address Comparator | 器件地址比较器（匹配 A2/A1/A0） |
| Data Recovery (LOAD/INC) | 数据恢复电路 |
| R/W Data Word Addr/Counter | 读写数据字地址计数器 |
| YDEC | 行译码器 |
| Serial MUX | 串行多路复用器 |
| D_IN / D_OUT / ACK Logic | 数据输入/输出/应答逻辑 |
| EEPROM Array | EEPROM 存储阵列（512 pages × 64 bytes） |
| CEDX | 列/行使能译码 |

外部引脚：VCC、GND、WP、SCL、SDA、A2、A1、A0

---

## 12. Revision History / 版本历史

> 参考 PDF 第 20 页，Section 12

| Document Rev. | Date | Description / 说明 |
|--------------|------|-------------------|
| 8568A | 09/2009 | Initial document release / 初版发布 |
| 8568B | 05/2010 | Updated tAA and tDH parameters; updated tR and tF for 1.7V operation; corrected device description / 更新 tAA 和 tDH 参数；更新 1.7V 工作时的 tR 和 tF；修正器件描述 |
| 8568C | 06/2010 | Updated DC and AC characteristics tables; corrected write protect table / 更新直流和交流特性表；修正写保护表 |
| 8568D | 01/2011 | Updated Section 10 (Packaging) / 更新第 10 节（封装）内容 |
| 8568E | 08/2012 | Updated pin capacitance table; added VFBGA package / 更新引脚电容表；新增 VFBGA 封装 |

---

*End of Extract / 提取完毕*
