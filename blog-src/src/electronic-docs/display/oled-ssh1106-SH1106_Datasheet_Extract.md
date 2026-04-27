# SH1106 Datasheet Extract
# SH1106 数据手册提取

**Source / 来源:** SH1106_V2.3.pdf（49 pages）  
**Extracted / 提取日期:** 2026-04-25  
**Scope / 范围:** 全文技术内容（双语，英文原文 + 中文注释）

---

## Table of Contents / 目录

1. [General Description / 总体描述](#1-general-description--总体描述)
2. [Block Diagram / 功能框图](#2-block-diagram--功能框图)
3. [Pad Description / 引脚定义](#3-pad-description--引脚定义)
4. [Pad Configuration / 引脚布局](#4-pad-configuration--引脚布局)
5. [MPU Interface / MCU 接口](#5-mpu-interface--mcu-接口)
6. [I2C Protocol / I²C 协议](#6-i2c-protocol--i²c-协议)
7. [Functional Description / 功能描述](#7-functional-description--功能描述)
8. [Commands / 指令集](#8-commands--指令集)
9. [Power On/Off Sequence / 上下电时序](#9-power-onoff-sequence--上下电时序)
10. [Absolute Maximum Ratings / 绝对最大额定值](#10-absolute-maximum-ratings--绝对最大额定值)
11. [Electrical Characteristics / 电气特性](#11-electrical-characteristics--电气特性)
12. [AC Characteristics / 交流特性（时序参数）](#12-ac-characteristics--交流特性时序参数)
13. [Application Circuits / 典型应用电路](#13-application-circuits--典型应用电路)
14. [Revision History / 版本历史](#14-revision-history--版本历史)

---

## 1. General Description / 总体描述

*(PDF Page 1)*

SH1106 is a single-chip CMOS OLED/PLED driver with controller for organic/polymer light emitting diode dot-matrix graphic display system. SH1106 consists of 132 segments, 64 commons that can support a maximum display resolution of 132 X 64. It is designed for Common Cathode type OLED panel.

SH1106 embeds with contrast control, display RAM oscillator and efficient DC-DC converter, which reduces the number of external components and power consumption. SH1106 is suitable for a wide range of compact portable applications, such as sub-display of mobile phone, calculator and MP3 player, etc.

> **中文注释：**  
> SH1106 是单芯片 CMOS OLED/PLED 显示驱动控制器，支持最大 132×64 点阵分辨率，适用于共阴极（Common Cathode）OLED 面板。  
> 内置对比度控制、显示 RAM、振荡器和高效 DC-DC 升压电路，减少外部元器件数量和功耗。  
> 适用于手机子屏、计算器、MP3 播放器等紧凑型便携设备。

### Features / 特性列表

*(PDF Page 1)*

| Feature | Description |
|---------|-------------|
| Display resolution | Support maximum 132 × 64 dot matrix panel |
| Display RAM | Embedded 132 × 64 bits SRAM |
| Logic supply (VDD1) | 1.65V – 3.5V |
| DC-DC supply (VDD2) | 3.0V – 4.2V |
| OLED supply (VPP) external | 6.4V – 14.0V |
| OLED supply (VPP) internal | 6.4V – 9.0V (internal charge pump) |
| Max segment output current | 200 mA |
| Max common sink current | 27 mA |
| Interface types | 8-bit 6800-series parallel, 8-bit 8080-series parallel, 3-wire SPI, 4-wire SPI, 400 kHz fast I²C |
| Frame frequency | Programmable |
| Multiplexing ratio | Programmable |
| Column/Row re-mapping | ADC (segment) + vertical scrolling |
| Oscillator | On-chip RC oscillator |
| Charge pump | Programmable internal charge pump output |
| Contrast control | 256-step contrast control |
| Low power | Sleep mode: <5 mA; VDD1=0V, VDD2=3.0–4.2V: <5 mA; VDD1,2=0V, VPP=6.4–14.0V: <5 mA |
| Temperature range | –40°C to +85°C |
| Package | COG (Chip-On-Glass) form, thickness: 300 μm |

> **中文注释：**  
> - VDD1：逻辑电源，1.65–3.5V  
> - VDD2：DC-DC 电荷泵电源，3.0–4.2V  
> - VPP：OLED 面板驱动电压，外部供电 6.4–14V，内部升压 6.4–9V  
> - ADC：列地址/行方向重映射，用于灵活的模块安装方向  
> - COG：玻璃上芯片封装，厚度 300 μm

---

## 2. Block Diagram / 功能框图

*(PDF Page 2 — See original PDF for diagram)*

**Figure reference: PDF Page 2, Block Diagram**

> **中文注释：**  
> 功能框图请参见 PDF 第 2 页。主要模块如下：
> - **Display Data RAM（显示数据 RAM）**：132×64 bits 静态 RAM，存储显示位图
> - **Column address decoder（列地址译码器）**：解析列地址，控制 SEG 输出
> - **Page Address Register（页地址寄存器）**：管理页地址（共 8 页）
> - **Display Timing Generator（显示时序发生器）**：产生 CL 时钟
> - **Power supply circuit（电源电路）**：处理 VDD1、VDD2、VPP、VCOMH 等
> - **Charge Pump（电荷泵）**：C1N/C1P/C2N/C2P 外接电容
> - **Segment driver（段驱动器）**：SEG0–SEG131，电流源驱动
> - **Common driver（公共驱动器）**：COM0–COM63，电压扫描
> - **Microprocessor Interface（MCU 接口）**：支持 CS、A0、RD、WR、RES、IM0/1/2、D0–D7
> - **Command Decoder（命令译码器）**：解析命令字节

---

## 3. Pad Description / 引脚定义

*(PDF Pages 3–5)*

### 3.1 Power Supply Pads / 电源引脚

| Symbol | I/O | Description |
|--------|-----|-------------|
| VDD1 | Supply | Power supply input: **1.65 – 3.5V**. Logic supply. |
| VDD2 | Supply | **3.0 – 4.2V** power supply pad for charge pump circuit. **Disconnect when VPP is supplied externally.** |
| VSS | Supply | Ground. |
| VSL | Supply | Segment voltage reference pad. **Connect to VSS externally.** |
| VCL | Supply | Common voltage reference pad. **Connect to VSS externally.** |

> **中文注释：**  
> - VDD1：逻辑供电，1.65–3.5V  
> - VDD2：电荷泵供电，3.0–4.2V。**使用外部 VPP 时必须断开 VDD2**  
> - VSS：地  
> - VSL：段驱动电压基准，需在外部连接到 VSS  
> - VCL：公共驱动电压基准，需在外部连接到 VSS

### 3.2 OLED Driver Supply Pads / OLED 驱动电源引脚

| Symbol | I/O | Description |
|--------|-----|-------------|
| IREF | O | Segment current reference pad. **Connect a resistor between IREF and VSS. Set current at 12.5 mA.** |
| VCOMH | O | Voltage output high level for common signals. **Connect a capacitor between VCOMH and VSS.** |
| VBREF | NC | Internal voltage reference pad for booster circuit. **Keep floating.** |
| VPP | P | OLED panel power supply. Generated by internal charge pump. **Connect to capacitor. Can be supplied externally.** |
| C1N, C1P | P | Charge pump capacitor connections. **Not used and should be disconnected when VPP is supplied externally.** |
| C2P, C2N | P | Charge pump capacitor connections. **Not used and should be disconnected when VPP is supplied externally.** |

> **中文注释：**  
> - IREF：段电流基准，外接电阻到 VSS，设定 12.5mA 参考电流（R≈510kΩ）  
> - VCOMH：COM 高电平输出，外接滤波电容  
> - VBREF：升压电路内部基准，**悬空不连**  
> - VPP：OLED 面板驱动电源，内部升压或外部供电，需接滤波电容  
> - C1N/C1P、C2N/C2P：电荷泵飞跨电容。**使用外部 VPP 时断开**

### 3.3 System Bus Connection Pads / 系统总线引脚

*(PDF Page 4)*

| Symbol | I/O | Description |
|--------|-----|-------------|
| CL | I/O | System clock input/output. **When internal clock enabled (CLS="H"), leave open; internal clock is output here. When CLS="L", connect external clock source here.** |
| CLS | I | Internal clock enable. CLS="H": internal oscillator enabled. CLS="L": internal oscillator disabled, external clock required on CL. |
| IM0, IM1, IM2 | I | MPU interface mode select. See interface mode table below. |
| CS | I | Chip select, active LOW. When CS="L", chip is selected and data/command I/O is enabled. |
| RES | I | Reset signal, active LOW. When RES="L", settings are initialized to default state. |
| A0 | I | Data/Command control. A0="H": D0–D7 treated as display data. A0="L": D0–D7 treated as command. **In I²C mode, this pad serves as SA0 (slave address LSB).** |
| WR (R/W) | I | 8080: active LOW write strobe; data latched at rising edge. 6800: R/W control, "H"=Read, "L"=Write. |
| RD (E) | I | 8080: active LOW read enable; data bus in output mode when "L". 6800: active HIGH enable clock. |
| D0–D7 | I/O | 8-bit bidirectional data bus. |

> **中文注释：**  
> - CL/CLS：时钟控制。CLS 高：使用内部 RC 振荡器，CL 悬空；CLS 低：外部时钟输入到 CL  
> - IM0/1/2：接口模式选择，见下表  
> - CS：片选，低有效  
> - RES：复位，低有效，复位时间 ≥10μs  
> - A0：数据/命令选择；I²C 模式下为从机地址最低位 SA0  
> - WR/RD：写/读控制，在不同接口模式下含义不同

**Interface Mode Selection / 接口模式选择表**

*(PDF Page 4)*

| IM2 | IM1 | IM0 | Interface Mode |
|-----|-----|-----|----------------|
| 1 | 1 | 0 | 8080-series 8-bit parallel |
| 1 | 0 | 1 | 6800-series 8-bit parallel |
| 0 | 0 | 0 | 4-wire SPI |
| 0 | 0 | 1 | 3-wire SPI |
| 0 | 1 | 0 | I²C |

> **中文注释：**  
> 通过 IM0/IM1/IM2 三个引脚硬件配置接口类型，上电前必须设置好，不可软件更改

### 3.4 OLED Drive Pads / OLED 驱动输出引脚

*(PDF Page 5)*

| Symbol | I/O | Description |
|--------|-----|-------------|
| COM0, 2, –60, 62 | O | Even Common signal outputs for OLED display. |
| COM1, 3–61, 63 | O | Odd Common signal outputs for OLED display. |
| SEG0–131 | O | Segment signal outputs for OLED display. |

> **中文注释：**  
> - COM0–COM63：64 路公共端驱动输出（偶数和奇数分开排列）  
> - SEG0–SEG131：132 路段驱动电流源输出

### 3.5 Test Pads / 测试引脚

*(PDF Page 5)*

| Symbol | I/O | Description |
|--------|-----|-------------|
| TEST1–3 | I | Test pads, internal pull-low. **No connection for user.** |
| Dummy | — | Unused pads. **Keep floating.** |

> **中文注释：**  
> 测试引脚内部下拉，用户不连接。Dummy 引脚悬空处理。

---

## 4. Pad Configuration / 引脚布局

*(PDF Pages 6–7)*

### Chip Outline Dimensions / 芯片外形尺寸

| Item | Pad No. | Size X (μm) | Size Y (μm) |
|------|---------|-------------|-------------|
| Chip boundary | — | 5076 | 814 |
| Chip height (all pads) | — | — | 300 |
| Bump size (I/O) | — | 40 | 80 |
| Bump size (SEG) | — | 15 | 110 |
| Bump size (COM) | — | 110 | 15 |
| Pad pitch (COM) | — | 30 μm | — |
| Pad pitch (SEG) | — | 30.75 μm | — |
| Pad pitch (I/O) | — | 55 μm | — |
| Bump height (all pads) | — | 9±2 μm | — |

> **中文注释：**  
> - 芯片尺寸：5076μm × 814μm（含封装厚度 300μm）  
> - I/O bump：40×80μm；SEG bump：15×110μm；COM bump：110×15μm  
> - bump 高度：9±2μm

### Alignment Mark Location / 对准标记位置

| Mark | X (μm) | Y (μm) |
|------|--------|--------|
| ALK_L | –2470 | –348 |
| ALK_R | +2470 | –348 |

> **中文注释：**  
> 对准标记用于 COG 贴装定位，左右各一个，关于芯片中心对称

### Total Pad Count / 引脚总数

Total: **266 pads** (PDF Page 7 — Full pad location table with X/Y coordinates in μm, see original PDF for complete listing)

> **中文注释：**  
> 共 266 个焊盘，完整坐标表格（含每个 pad 的 X/Y 位置，单位 μm）请参见 PDF 第 7 页。用于 COG 贴装的精确定位。

---

## 5. MPU Interface / MCU 接口

*(PDF Pages 8–12)*

> **中文注释：**  
> SH1106 支持 5 种接口，通过 IM0/IM1/IM2 引脚配置。以下分别描述各接口信号操作。

### 5.1 8080-Series Parallel Interface / 8080 并行接口

*(PDF Page 8)*

The 8080 MPU write operation is performed by pulling WR signal LOW (A0 defines data or command). Data is latched on the rising edge of WR. The read operation is performed by pulling RD LOW; data bus goes to output state.

**Signal mapping:**

| SH1106 Pin | 8080 MPU Signal |
|------------|----------------|
| WR | WR (active LOW) |
| RD | RD (active LOW) |
| A0 | A0 (D/C) |
| CS | CS (active LOW) |
| D0–D7 | Data bus |

> **中文注释：**  
> 8080 接口写操作：WR 拉低，A0 选择数据/命令，数据在 WR 上升沿锁存。读操作：RD 拉低，数据总线输出数据。

### 5.2 6800-Series Parallel Interface / 6800 并行接口

*(PDF Page 8–9)*

The 6800 MPU write operation is performed when R/W is LOW and E (enable) is HIGH. The read operation is performed when R/W is HIGH and E is HIGH.

**Signal mapping:**

| SH1106 Pin | 6800 MPU Signal |
|------------|----------------|
| WR | R/W |
| RD | E (Enable, active HIGH) |
| A0 | A0 (D/C) |
| CS | CS (active LOW) |
| D0–D7 | Data bus |

> **中文注释：**  
> 6800 接口写操作：R/W 低 + E 高触发写入。读操作：R/W 高 + E 高触发读取。与 8080 接口的主要区别是使用 E（Enable）代替独立的 RD/WR。

### 5.3 4-Wire SPI Interface / 4线 SPI 接口

*(PDF Page 10)*

- **D1 (SI):** Serial data input (MOSI)
- **D0 (SCL):** Serial clock input
- **A0:** Data/Command select (D/C)
- **CS:** Chip select, active LOW
- **WR, RD:** Not used, should be fixed to VSS or VDD1
- **D2–D7:** Not used, fix to VSS

Data is shifted in MSB first on the rising edge of SCL. A0 defines whether the byte is a command (A0=L) or data (A0=H).

> **中文注释：**  
> 4线 SPI：D1=MOSI（数据），D0=SCL（时钟），A0=DC（数据/命令），CS=片选。  
> 数据 MSB 先入，SCL 上升沿采样。WR、RD 不使用，接 VSS 或 VDD1。D2–D7 接 VSS。

### 5.4 3-Wire SPI Interface / 3线 SPI 接口

*(PDF Page 10–11)*

- **D1 (SI):** Serial data input (MOSI) — 9-bit transmission (1 D/C bit + 8 data bits)
- **D0 (SCL):** Serial clock input
- **CS:** Chip select, active LOW
- **A0:** Not used in 3-wire SPI, fix to VSS
- **WR, RD:** Not used, fix to VSS or VDD1

In 3-wire SPI mode, the D/C bit is prepended as the first bit of each 9-bit transfer (MSB first).

> **中文注释：**  
> 3线 SPI：每次发送 9 位，第1位是 D/C（0=命令，1=数据），后跟 8 位数据内容。  
> 省去了 A0 引脚（固定接 VSS），以 9 位帧代替 A0 信号区分命令/数据。

### 5.5 I²C Interface / I²C 接口

*(PDF Pages 11–13)*

- **D1 (SDA):** Serial data (bidirectional)
- **D0 (SCL):** Serial clock input
- **A0 (SA0):** Slave address LSB — SA0="L" (VSS): address = 0111100b (0x3C); SA0="H" (VDD1): address = 0111101b (0x3D)
- **CS:** Fix to VSS in I²C mode
- **WR, RD:** Not used, fix to VSS or VDD1
- **D2–D7:** Not used, fix to VSS (except D1 used as SDA)
- Max bus speed: **400 kHz (Fast Mode)**
- Pull-up resistor supply must equal VDD1

**I²C Slave Addresses:**

| SA0 | 7-bit Slave Address | 8-bit Write | 8-bit Read |
|-----|---------------------|-------------|------------|
| 0 (VSS) | 0111100b | 0x78 | 0x79 |
| 1 (VDD1) | 0111101b | 0x7A | 0x7B |

> **中文注释：**  
> I²C 接口，最高 400kHz。从机地址由 SA0（即 A0 引脚）决定：  
> - SA0=0：地址 0x3C（写 0x78，读 0x79）  
> - SA0=1：地址 0x3D（写 0x7A，读 0x7B）  
> 上拉电阻供电必须等于 VDD1。

**I²C Protocol / I²C 通信协议**

*(PDF Pages 12–13)*

The SH1106 I²C protocol uses **control byte + data byte** pairs:

- **Control byte** contains:
  - **Co bit (bit 7):** Continuation bit. Co=1: another control byte follows; Co=0: only data bytes follow after this control byte.
  - **D/C bit (bit 6):** D/C=0: following data bytes are commands; D/C=1: following data bytes are written to display RAM.
  - **Bits 5–0:** Always 0

- **Data byte:** Command code or display data (depending on D/C bit)

**Transmission sequence:**
1. START condition (S)
2. Slave address + R/W bit
3. ACK
4. Control byte (Co + D/C + 000000)
5. ACK
6. Data byte(s)
7. ACK after each byte
8. STOP condition (P)

> **中文注释：**  
> I²C 帧结构：  
> 1. 起始条件 S  
> 2. 从机地址（7位）+ R/W 位  
> 3. ACK  
> 4. 控制字节：[Co][D/C][0][0][0][0][0][0]  
>    - Co=1：后续还有控制字节；Co=0：后续只有数据字节  
>    - D/C=0：后续数据为命令；D/C=1：后续数据写入显示 RAM  
> 5. ACK  
> 6. 数据字节（可以连续多个）  
> 7. 每字节后 ACK  
> 8. 停止条件 P  
>
> **典型写命令：** S → 0x78(写) → ACK → 0x00(Co=0,D/C=0) → ACK → [命令字节] → ACK → P  
> **典型写数据：** S → 0x78(写) → ACK → 0x40(Co=0,D/C=1) → ACK → [数据字节×N] → ACK → P

---

## 6. I2C Protocol / I²C 协议（补充）

*(PDF Page 12 — Figure 6: Acknowledge, Figure 7: I²C Bus Protocol)*

**Figure reference: PDF Page 12, Figure 6 (Acknowledge), Figure 7 (I²C Bus Protocol)**

> **中文注释：**  
> I²C ACK 机制：主机发送每个字节后，从机（SH1106）在第 9 个时钟脉冲期间拉低 SDA 表示应答（ACK）。  
> 若主机不产生 ACK（NACK），表示数据读取结束，发送方停止传输数据。

---

## 7. Functional Description / 功能描述

*(PDF Pages 14–18)*

### 7.1 Display Data RAM / 显示数据 RAM

*(PDF Page 14)*

The Display Data RAM is a bit mapped static RAM holding the bit pattern to be displayed. The size of the RAM is **132 × 64 bits**.

For mechanical flexibility, re-mapping on both segment and common outputs can be selected by software.

For vertical scrolling, an internal register storing display start line can be set to control the portion of the RAM data mapped to the display.

> **中文注释：**  
> 显示数据 RAM 是 132×64 位静态 RAM，存储显示位图。  
> - 列（段）方向和行（公共端）方向均可软件重映射，便于灵活安装方向  
> - 显示起始行可通过寄存器设置，实现垂直滚动功能

### 7.2 Page Address Circuit / 页地址电路

*(PDF Page 15)*

The page address of the display data RAM is specified through the **Page Address Set Command**. The page address must be re-specified when changing pages. There are **8 pages (PAGE0–PAGE7)**, each page corresponds to 8 rows (8 COM lines).

> **中文注释：**  
> 显示 RAM 按页组织，共 8 页（PAGE0–PAGE7），每页 8 行。  
> 切换页面时必须重新设置页地址。页地址不会自动递增。

### 7.3 Column Address / 列地址

*(PDF Page 15)*

The display data RAM column address is specified by the **Column Address Set command**. The specified column address is **incremented (+1) with each display data read/write command**, allowing continuous MPU access.

**Note:** Column address is independent of page address. When moving from page 0 column 83H to page 1 column 00H, both page address and column address must be re-specified.

**Column re-mapping (ADC):**

*(PDF Page 15, Table 7)*

| ADC Setting | SEG0 | SEG131 |
|-------------|------|--------|
| ADC="0" | Column Address 00H → | → Column Address 83H |
| ADC="1" | Column Address 83H ← | ← Column Address 00H |

> **中文注释：**  
> 列地址每次读写数据后自动 +1，支持连续写入。  
> 页地址不自动增加，跨页时需手动重设页地址和列地址。  
> ADC 命令（A0H/A1H）控制列地址与 SEG 输出的映射方向，用于翻转屏幕水平方向。

### 7.4 Line Address Circuit / 行地址电路

*(PDF Page 15)*

The line address circuit specifies the line address relating to the common output. The **display start line address set command** specifies which RAM line is displayed at the top:
- Normal common output mode: top line = COM0
- Reversed common output mode: top line = COM63

The display area is **64 lines** (COM0–COM63) for SH1106.

Dynamic changes to the start line address enable screen scrolling, page swapping, etc.

**RAM Address → Common Output mapping (normal mode):**

*(PDF Page 16)*

| D3 D2 D1 D0 | Start Address | Common Output Mapping |
|-------------|---------------|-----------------------|
| 0 0 0 0 | 00H | COM0, COM1, … COM63 |
| 0 0 0 1 | 01H | COM1, COM2, … COM0 (wrap) |
| 0 0 1 0 | 02H | COM2, COM3, … |
| … | … | … |
| 0 1 1 1 | 07H | COM7, COM8, … |
| … | … | … |
| 0 0 1 1 1 1 1 1 | 3FH | COM63, COM0, … |

> **中文注释：**  
> 行起始地址决定显示内容从 RAM 哪一行开始输出到 COM0（第一行）。  
> 地址范围 00H–3FH（对应 0–63），更改起始地址可实现垂直滚动效果（PDF 第 16 页有完整行地址/COM 映射表）。

### 7.5 Oscillator Circuit / 振荡器电路

*(PDF Page 17)*

This is a RC type oscillator that produces the display clock:
- CLS = "H": Internal oscillator enabled, outputs clock through CL terminal (leave CL open)
- CLS = "L": Internal oscillator stopped; external clock must be connected to CL terminal

The oscillator output feeds through a **DIVIDER** and **MUX** to produce the internal display clock (DCLK).

**Figure reference: PDF Page 17, Figure 9 (Oscillator Circuit)**

> **中文注释：**  
> RC 型片内振荡器，频率可通过命令 D5H 设置。  
> - CLS=高：使用内部振荡器，CL 引脚悬空  
> - CLS=低：内部振荡器停止，需从 CL 引脚输入外部时钟  
> 振荡器输出经分频和多路选择后生成显示时钟 DCLK

### 7.6 Charge Pump Regulator / 电荷泵稳压器

*(PDF Page 18)*

The charge pump block, together with only **2 external capacitors** (C1 and C2, 0.22 μF each), generates a **6.4V–9.0V** voltage for the OLED panel. It can be turned ON/OFF by software command **8Bh** setting.

**Charge Pump output voltage control:** Driving voltage is adjustable from 6.4V to 9.0V to meet different panel requirements.

**Reset Circuit default states:**

When RES is pulled LOW, the following defaults are set:
1. Display OFF; common and segment outputs in high-impedance state
2. 132 × 64 display mode
3. Normal segment and display data column/row address mapping (SEG0 → column address 00H; COM0 → row address 00H)
4. Shift register data cleared in serial interface
5. Display start line set to RAM line address 00H
6. Column address counter set to 0
7. Normal scanning direction of common outputs
8. Contrast control register set to **80H**
9. Internal DC-DC selected

> **中文注释：**  
> 电荷泵仅需 2 个外部电容（C1、C2，各 0.22μF）即可将 VDD2 升压到 6.4–9V 供 OLED 使用。  
> 命令 8Bh 控制电荷泵开关：8Bh + 8Bh（ON）或 8Bh + 8Ah（OFF）。  
>  
> **复位默认状态（RES="L"时）：**  
> - 显示关闭，所有输出高阻  
> - 正常地址映射（SEG0↔00H，COM0↔00H）  
> - 列地址计数器=0  
> - 对比度=0x80  
> - 使用内部 DC-DC

---

## 8. Commands / 指令集

*(PDF Pages 19–31)*

> **中文注释：**  
> SH1106 通过 A0、RD(E)、WR(R/W) 信号区分命令/数据/读写。  
> 芯片使用内部时钟处理命令，处理速度极快，通常**不需要 BUSY 检测**。  
> 命令字节通过 A0=L（命令模式）发送。

### 8.1 Command Table / 指令汇总表

*(PDF Pages 20–31)*

| # | Command | A0 | RD | WR | D7 | D6 | D5 | D4 | D3 | D2 | D1 | D0 | POR | Description |
|---|---------|----|----|-----|----|----|----|----|----|----|----|----|-----|-------------|
| 1a | Set Lower Column Address | 0 | 1 | 0 | 0 | 0 | 0 | 0 | X3 | X2 | X1 | X0 | 00H | Set lower nibble of column address. D[3:0] = lower 4 bits of column address. |
| 1b | Set Higher Column Address | 0 | 1 | 0 | 0 | 0 | 0 | 1 | X3 | X2 | X1 | X0 | 10H | Set higher nibble of column address. D[3:0] = upper 4 bits. Column addr = (Higher&lt;&lt;4) OR Lower. Range: 00H–83H. |
| 2 | Set Pump Voltage Value | 0 | 1 | 0 | 0 | 0 | 1 | 1 | 0 | 0 | V1 | V0 | 32H | Set internal charge pump output voltage. V[1:0]: 00=6.4V, 01=7.4V, 10=8.0V (POR), 11=9.0V |
| 3 | Set Display Start Line | 0 | 1 | 0 | 0 | 1 | A5 | A4 | A3 | A2 | A1 | A0 | 40H | Set display RAM start line address (0–63). Range: 40H–7FH. |
| 4 | Set Contrast Control | 0 | 1 | 0 | 1 | 0 | 0 | 0 | 0 | 0 | 0 | 1 | 81H | Double byte command. First byte: 81H (mode set). |
| 4b | Contrast Data | 0 | 1 | 0 | A7 | A6 | A5 | A4 | A3 | A2 | A1 | A0 | 80H | Second byte: 8-bit contrast value (00H–FFH). Higher value = brighter. POR=80H. |
| 5 | Set Segment Re-map (ADC) | 0 | 1 | 0 | 1 | 0 | 1 | 0 | 0 | 0 | 0 | D | A0H | D=0: SEG0→col 00H (normal, A0H). D=1: SEG131→col 00H (reverse, A1H). POR=A0H. |
| 6 | Set Entire Display | 0 | 1 | 0 | 1 | 0 | 1 | 0 | 0 | 1 | 0 | D | A4H | D=0: display follows RAM content (A4H). D=1: all pixels ON regardless of RAM (A5H). POR=A4H. |
| 7 | Set Normal/Reverse Display | 0 | 1 | 0 | 1 | 0 | 1 | 0 | 0 | 1 | 1 | D | A6H | D=0: normal (A6H, lit pixel = RAM 1). D=1: reverse (A7H, lit pixel = RAM 0). POR=A6H. |
| 8a | Set Multiplex Ratio (Mode Set) | 0 | 1 | 0 | 1 | 0 | 1 | 0 | 1 | 0 | 0 | 0 | A8H | Double byte. First byte: A8H. |
| 8b | Multiplex Ratio Data | 0 | 1 | 0 | * | * | A5 | A4 | A3 | A2 | A1 | A0 | 3FH | Second byte: multiplex ratio (00H–3FH, i.e., 1–64). POR=3FH (64 MUX). |
| 9a | DC-DC Control Mode Set | 0 | 1 | 0 | 1 | 0 | 1 | 0 | 1 | 1 | 0 | 1 | ADH | Double byte. First byte: ADH. |
| 9b | DC-DC ON/OFF | 0 | 1 | 0 | 1 | 0 | 0 | 0 | 1 | 0 | 1 | D | 8BH | D=1: DC-DC ON (8BH). D=0: DC-DC OFF (8AH). POR=8BH. |
| 10 | Display OFF/ON | 0 | 1 | 0 | 1 | 0 | 1 | 0 | 1 | 1 | 1 | D | AEH | D=0: display OFF (AEH, sleep mode). D=1: display ON (AFH). POR=AEH. |
| 11 | Set Page Address | 0 | 1 | 0 | 1 | 0 | 1 | 1 | PA3 | PA2 | PA1 | PA0 | B0H | Set page address (0–7). Range: B0H–B7H. |
| 12 | Set Common Scan Direction | 0 | 1 | 0 | 1 | 1 | 0 | 0 | D | * | * | * | C0H | D=0: scan COM0→COM[N-1] (C0H, normal). D=1: scan COM[N-1]→COM0 (C8H, reverse). POR=C0H. |
| 13a | Display Offset Mode Set | 0 | 1 | 0 | 1 | 1 | 0 | 1 | 0 | 0 | 1 | 1 | D3H | Double byte. First byte: D3H. |
| 13b | Display Offset Data Set | 0 | 1 | 0 | * | * | A5 | A4 | A3 | A2 | A1 | A0 | 00H | Map display start line to COM0–63. POR=00H. |
| 14a | Display Clock Divide Ratio/Osc Freq Mode Set | 0 | 1 | 0 | 1 | 1 | 0 | 1 | 0 | 1 | 0 | 1 | D5H | Double byte. First byte: D5H. POR=50H. |
| 14b | Divide Ratio/Osc Freq Data | 0 | 1 | 0 | F3 | F2 | F1 | F0 | D3 | D2 | D1 | D0 | 50H | [7:4]=oscillator frequency adjust; [3:0]=divide ratio (0=÷1, 1=÷2, …, F=÷16). POR=50H (freq=5, div=1). |
| 15a | Dis-charge/Pre-charge Period Mode Set | 0 | 1 | 0 | 1 | 1 | 0 | 1 | 1 | 0 | 0 | 1 | D9H | Double byte. First byte: D9H. POR=22H. |
| 15b | Dis/Pre-charge Period Data | 0 | 1 | 0 | D3 | D2 | D1 | D0 | P3 | P2 | P1 | P0 | 22H | [7:4]=discharge period (1–15 DCLKs); [3:0]=precharge period (1–15 DCLKs). POR=22H. |
| 16a | Common Pads HW Config Mode Set | 0 | 1 | 0 | 1 | 1 | 0 | 1 | 1 | 0 | 1 | 0 | DAH | Double byte. First byte: DAH. POR=12H. |
| 16b | Sequential/Alternative Mode Set | 0 | 1 | 0 | 0 | 0 | 0 | D | 0 | 0 | 1 | 0 | 12H | D=0: sequential COM pins (02H). D=1: alternative COM pins (12H). POR=12H. |
| 17a | VCOM Deselect Level Mode Set | 0 | 1 | 0 | 1 | 1 | 0 | 1 | 1 | 0 | 1 | 1 | DBH | Double byte. First byte: DBH. POR=35H. |
| 17b | VCOM Deselect Level Data | 0 | 1 | 0 | — | — | β4 | β3 | β2 | β1 | β0 | — | 35H | VCOM = β × VREF. See voltage table. POR=35H. |
| 18 | Read-Modify-Write | 0 | 1 | 0 | 1 | 1 | 1 | 0 | 0 | 0 | 0 | 0 | — | E0H. Starts Read-Modify-Write mode. Column address not incremented on read. |
| 19 | End | 0 | 1 | 0 | 1 | 1 | 1 | 0 | 1 | 1 | 1 | 0 | — | EEH. Ends Read-Modify-Write mode. Returns column address to value before RMW started. |
| 20 | NOP | 0 | 1 | 0 | 1 | 1 | 1 | 0 | 0 | 0 | 1 | 1 | — | E3H. No operation. |
| 21 | Write Display Data | 1 | 1 | 0 | — | — | — | — | — | — | — | — | — | Write 8-bit data to display RAM at current column/page address. Column address auto-increments. |
| 22 | Read Status | 0 | 0 | 1 | BUSY | * | * | * | 0 | 0 | 0 | ON/OFF | — | Read BUSY flag (D7) and display ON/OFF status (D0). |
| 23 | Read Display Data | 1 | 0 | 1 | — | — | — | — | — | — | — | — | — | Read 8-bit data from display RAM at current column/page address. |

> **中文注释：**
>
> **常用命令速查：**
> - `AEH`：关闭显示（进入睡眠模式）
> - `AFH`：开启显示
> - `00H`–`0FH`：设置列地址低 4 位
> - `10H`–`1FH`：设置列地址高 4 位
> - `B0H`–`B7H`：设置页地址（0–7）
> - `40H`–`7FH`：设置显示起始行（0–63）
> - `81H` + 数据：设置对比度（00H–FFH，默认 80H）
> - `A0H/A1H`：列方向正常/翻转（ADC）
> - `A4H/A5H`：正常显示/全亮
> - `A6H/A7H`：正相/反相显示
> - `A8H` + 数据：设置多路复用比（1–64）
> - `ADH` + `8AH/8BH`：DC-DC 关/开
> - `C0H/C8H`：行扫描方向正常/翻转
> - `D3H` + 数据：显示偏移
> - `D5H` + 数据：时钟分频比 + 振荡器频率
> - `D9H` + 数据：预充电/放电时间
> - `DAH` + 数据：COM 引脚硬件配置
> - `DBH` + 数据：VCOM 去选择电平
> - `E0H`：进入读-改-写模式
> - `EEH`：退出读-改-写模式
> - `E3H`：NOP

### 8.2 Charge Pump Voltage Settings / 电荷泵电压设置

*(PDF Page 20)*

Command 3xH (Set Pump Voltage Value), bits V1:V0:

| V1 | V0 | VPP Output |
|----|----|-----------|
| 0 | 0 | 6.4V |
| 0 | 1 | 7.4V |
| 1 | 0 | 8.0V (POR) |
| 1 | 1 | 9.0V |

> **中文注释：**  
> 命令字节：30H=6.4V，31H=7.4V，32H=8.0V（上电默认），33H=9.0V

### 8.3 Vertical Scrolling / 垂直滚动

*(PDF Pages 21–22)*

Set Display Start Line command (40H–7FH) shifts the display window within the 64-row RAM:
- 40H: Display starts at RAM row 0 (default)
- 41H: Display starts at RAM row 1
- …
- 7FH: Display starts at RAM row 63

Dynamic update of this register creates smooth vertical scrolling.

> **中文注释：**  
> 通过快速更新显示起始行寄存器可实现平滑垂直滚动，不需要移动 RAM 数据本身。

### 8.4 VCOM Deselect Level / VCOM 去选择电平

*(PDF Page 28–29)*

Command DBH + data byte. VCOM = β × VREF.

Data byte bits [6:2] select the β value:

| Data Byte (hex) | β value | VCOM (typical, VREF=7.5V) |
|-----------------|---------|--------------------------|
| 00H | — | — |
| 20H | 0.430 | ~3.23V |
| 30H | 0.571 | ~4.28V |
| 35H (POR) | 0.638 | ~4.79V |
| 3DH | 0.750 | ~5.63V |

*(Full table in PDF Page 29)*

> **中文注释：**  
> VCOMH 去选择电压影响对比度和残影，通常配合对比度命令一起调整。  
> 完整的 β 值对应表请参见 PDF 第 29 页。

### 8.5 Read-Modify-Write Mode / 读-改-写模式

*(PDF Page 30)*

The **Read-Modify-Write** (E0H) and **End** (EEH) commands bracket a read-then-modify-then-write operation:
- After E0H: each **read** does NOT increment the column address; each **write** increments it normally
- EEH returns the column address to the value it had when E0H was issued

> **中文注释：**  
> 读-改-写模式用于对 RAM 内容进行位操作（如光标闪烁、XOR 操作）。  
> 进入 RMW 后：读不移动列地址，写才自动 +1，退出时列地址恢复到 RMW 前的值。

---

## 9. Power On/Off Sequence / 上下电时序

*(PDF Pages 32–34)*

### 9.1 Power On with Built-in DC-DC Pump / 内部电荷泵上电时序

*(PDF Page 32)*

**Figure reference: PDF Page 32, Power on sequence diagram (internal DC-DC)**

Steps:
1. VPP is off
2. Turn on VDD1 and VDD2 power
3. Keep RES pin = "L" (>10 μs)
4. Release reset: RES pin = "H"
5. Reset timing: refer to reset timing spec (tR ≤ 2ms at VDD1=1.65–3.5V; tRW ≥ 10ms)
6. Initialized state (default registers loaded)
7. Set up initial code (user configuration commands)
8. Clear internal RAM to 00H
9. Set display on: command **AFH**
10. Wait 100 ms
11. Send display data

> **中文注释：**  
> **内部 DC-DC 上电流程：**  
> 1. VDD1 + VDD2 上电（VPP 暂不接外部电源，由内部电荷泵生成）  
> 2. RES 保持低电平 ≥10μs  
> 3. 释放复位（RES="H"），等待内部复位完成（≤2ms）  
> 4. 发送初始化命令序列  
> 5. 清 RAM（发送全 00H）  
> 6. 发送 AFH 开显示  
> 7. 等待 100ms  
> 8. 开始发送显示数据

### 9.2 Power On with External VPP / 外部 VPP 上电时序

*(PDF Page 33)*

**Figure reference: PDF Page 33, Power on sequence diagram (external VPP)**

Steps:
1. Turn on VDD1 and external VPP power simultaneously
2. Keep RES pin = "L" (>10 μs)
3. Release reset: RES pin = "H"
4. Reset timing: same as above
5. Initialized state (default registers loaded)
6. Set up initial code
7. Clear internal RAM to 00H
8. Set display on: **AFH**
9. Wait 100 ms
10. Send display data

> **中文注释：**  
> **外部 VPP 上电流程：** VDD1 与外部 VPP 同时上电（VDD2 不连接）。其余步骤与内部 DC-DC 模式相同。  
> **注意：** 使用外部 VPP 时 VDD2 必须断开（floating 或 NC）。

### 9.3 Power Off Sequence / 下电时序

*(PDF Page 34)*

**Figure reference: PDF Page 34, Power off sequence diagram**

> **中文注释：**  
> 下电流程请参见 PDF 第 34 页图示。  
> **注意（原文）：** "There will be no damages to the display module if the power sequences are not met."  
> 即：未严格遵守上下电时序不会损坏模块（该芯片对时序要求较宽松）。

---

## 10. Absolute Maximum Ratings / 绝对最大额定值

*(PDF Page 35)*

> **⚠️ 中文注释：** 超过以下额定值可能导致器件永久损坏。这些是压力测试额定值，不代表正常工作条件。

| Parameter | Value |
|-----------|-------|
| DC Supply Voltage VDD1 | –0.3V to +3.6V |
| DC Supply Voltage VDD2 | –0.3V to +4.3V |
| DC Supply Voltage VPP | –0.3V to +14.5V |
| Input Voltage | –0.3V to VDD1 + 0.3V |
| Operating Ambient Temperature | –40°C to +85°C |
| Storage Temperature | –55°C to +125°C |

---

## 11. Electrical Characteristics / 电气特性

*(PDF Pages 35–38)*

### 11.1 DC Characteristics / 直流特性

*(VSS = 0V, VDD1 = 1.65–3.5V, TA = +25°C, unless otherwise specified)*

| Symbol | Parameter | Min | Typ | Max | Unit | Condition |
|--------|-----------|-----|-----|-----|------|-----------|
| VDD1 | Operating voltage | 1.65 | — | 3.5 | V | — |
| VDD2 | Operating voltage | 3.0 | — | 4.2 | V | — |
| VPP | OLED operating voltage | 6.4 | — | 14.0 | V | — |
| IDD1 | Dynamic current consumption 1 | — | — | 110 | mA | VDD1=3V, VDD2=3.7V, IREF=12.5mA, Contrast=256, internal CP OFF, Display ON, all pixels ON, no panel |
| IDD2 | Dynamic current consumption 2 | — | — | 2 | mA | VDD1=3V, VDD2=3.7V, IREF=12.5mA, Contrast=256, internal CP ON, Display ON, all pixels ON, no panel |
| IPP | OLED dynamic current consumption | — | — | 1.5 | mA | VDD1=3V, VDD2=3.7V, VPP=9V(ext), IREF=12.5mA, Contrast=256, Display ON, all pixels ON, no panel |
| ISP (VDD1&2) | Sleep mode current in VDD1 & VDD2 | — | — | 5 | mA | VDD1=3V, VDD2=3.7V, TA=+25°C |
| ISP (VPP) | Sleep mode current in VPP | — | — | 5 | mA | VPP=9V (ext), TA=+25°C |
| ISEG | Segment output current | — | –200 | — | mA | VDD1=3V, VPP=9V, IREF=12.5mA, RLOAD=20kΩ, Contrast=256 |
| ISEG | Segment output current | — | –25 | — | mA | VDD1=3V, VPP=9V, IREF=12.5mA, RLOAD=20kΩ, Contrast=32 |
| ΔISEG1 | Segment current uniformity | — | — | ±3 | % | ΔISEG1=(ISEG-IMID)/IMID×100%; IMID=(IMAX+IMIN)/2; all 132 segments, Contrast=256 |
| ΔISEG2 | Adjacent segment current uniformity | — | — | ±2 | % | ΔISEG2=(ISEG[N]-ISEG[N+1])/(ISEG[N]+ISEG[N+1])×100%; all 132 segments, Contrast=256 |

> **中文注释：**  
> - IDD1/IDD2 测试均在无面板条件下进行  
> - ISEG 最大 200mA（对比度=256），最小 25mA（对比度=32）  
> - 段电流均匀性 ±3%（整体），相邻段电流差异 ±2%

*(PDF Page 36 — Additional DC Characteristics)*

| Symbol | Parameter | Min | Typ | Max | Unit | Condition |
|--------|-----------|-----|-----|-----|------|-----------|
| VIH | High-level input voltage | 0.8×VDD1 | — | VDD1 | V | — |
| VIL | Low-level input voltage | –0.3 | — | 0.2×VDD1 | V | — |
| VOH | High-level output voltage | 0.8×VDD1 | — | — | V | IOH = –100 μA |
| VOL | Low-level output voltage | — | — | 0.2×VDD1 | V | IOL = 100 μA |
| ILI | Input leakage current | — | — | ±1 | μA | VIN = VDD1 or VSS |

> **中文注释：**  
> - VIH（逻辑高电平输入）：≥ 0.8×VDD1  
> - VIL（逻辑低电平输入）：≤ 0.2×VDD1  
> - 输入漏电流 ≤ ±1μA

---

## 12. AC Characteristics / 交流特性（时序参数）

*(PDF Pages 37–44)*

> **中文注释：**  
> 以下所有时序参数测试条件：VDD1 = 1.65–3.5V，TA = +25°C，除非另有说明。  
> 时序波形图请参见对应 PDF 页面。

### 12.1 8080-Series Interface Timing / 8080 接口时序

*(PDF Page 37)*

**Figure reference: PDF Page 37, 8080 Interface Timing Diagram**

| Symbol | Parameter | Min | Typ | Max | Unit |
|--------|-----------|-----|-----|-----|------|
| tCYC | Write cycle time | 300 | — | — | ns |
| tAS | Address setup time (A0) | 0 | — | — | ns |
| tAH | Address hold time (A0) | 0 | — | — | ns |
| tDSW | Data setup time (write) | 40 | — | — | ns |
| tDHW | Data hold time (write) | 10 | — | — | ns |
| tWLW | WR LOW pulse width | 60 | — | — | ns |
| tWHW | WR HIGH pulse width | 60 | — | — | ns |
| tRCYC | Read cycle time | 300 | — | — | ns |
| tRLW | RD LOW pulse width | 120 | — | — | ns |
| tRHW | RD HIGH pulse width | 60 | — | — | ns |
| tDDR | Data output delay (read) | — | — | 120 | ns |
| tDHR | Data hold time (read) | 20 | — | — | ns |
| tR | Rise time | — | — | 15 | ns |
| tF | Fall time | — | — | 15 | ns |

### 12.2 6800-Series Interface Timing / 6800 接口时序

*(PDF Page 38)*

**Figure reference: PDF Page 38, 6800 Interface Timing Diagram**

| Symbol | Parameter | Min | Typ | Max | Unit |
|--------|-----------|-----|-----|-----|------|
| tCYC | Write/Read cycle time | 300 | — | — | ns |
| tAS | Address setup time | 0 | — | — | ns |
| tAH | Address hold time | 0 | — | — | ns |
| tDSW | Data setup time (write) | 40 | — | — | ns |
| tDHW | Data hold time (write) | 10 | — | — | ns |
| tELW | E LOW pulse width | 60 | — | — | ns |
| tEHW | E HIGH pulse width | 60 | — | — | ns |
| tDDR | Data output delay (read) | — | — | 120 | ns |
| tDHR | Data hold time (read) | 20 | — | — | ns |
| tR | Rise time | — | — | 15 | ns |
| tF | Fall time | — | — | 15 | ns |

### 12.3 4-Wire SPI Timing / 4线 SPI 时序

*(PDF Page 39)*

**Figure reference: PDF Page 39, 4-Wire SPI Timing Diagram**

| Symbol | Parameter | Min | Typ | Max | Unit |
|--------|-----------|-----|-----|-----|------|
| tCYC | Serial clock cycle | 100 | — | — | ns |
| tSHW | Serial clock HIGH pulse width | 40 | — | — | ns |
| tSLW | Serial clock LOW pulse width | 40 | — | — | ns |
| tAS | CS to SCL setup time | 10 | — | — | ns |
| tAH | SCL to CS hold time | 10 | — | — | ns |
| tDSW | Data setup time | 10 | — | — | ns |
| tDHW | Data hold time | 10 | — | — | ns |
| tR | Rise time | — | — | 15 | ns |
| tF | Fall time | — | — | 15 | ns |

### 12.4 3-Wire SPI Timing / 3线 SPI 时序

*(PDF Page 40)*

**Figure reference: PDF Page 40, 3-Wire SPI Timing Diagram**

*(Note: 9-bit transfer; D/C bit is first bit transmitted)*

| Symbol | Parameter | Min | Typ | Max | Unit |
|--------|-----------|-----|-----|-----|------|
| tCYC | Serial clock cycle | 100 | — | — | ns |
| tSHW | Serial clock HIGH pulse width | 40 | — | — | ns |
| tSLW | Serial clock LOW pulse width | 40 | — | — | ns |
| tDSW | Data setup time | 10 | — | — | ns |
| tDHW | Data hold time | 10 | — | — | ns |
| tR | Rise time | — | — | 15 | ns |
| tF | Fall time | — | — | 15 | ns |

### 12.5 I²C Interface Timing / I²C 接口时序

*(PDF Page 43)*

*(VDD1 = 1.65–3.5V, TA = +25°C)*

**Figure reference: PDF Page 43, I²C Timing Diagram**

| Symbol | Parameter | Min | Typ | Max | Unit |
|--------|-----------|-----|-----|-----|------|
| fSCL | SCL clock frequency | DC | — | 400 | kHz |
| TLOW | SCL clock LOW pulse width | 1.3 | — | — | μs |
| THIGH | SCL clock HIGH pulse width | 0.6 | — | — | μs |
| TSU:DATA | Data setup time | 100 | — | — | ns |
| THD:DATA | Data hold time | 0 | — | 0.9 | μs |
| TR | SCL, SDA rise time | 20+0.1Cb | — | 300 | ns |
| TF | SCL, SDA fall time | 20+0.1Cb | — | 300 | ns |
| Cb | Capacitive load on each bus line | — | — | 400 | pF |
| TSU:START | Setup time for re-START | 0.6 | — | — | μs |
| THD:START | START hold time | 0.6 | — | — | μs |
| TSU:STOP | Setup time for STOP | 0.6 | — | — | μs |
| TBUF | Bus free time between STOP and START | 1.3 | — | — | μs |

> **中文注释：**  
> - I²C 最高 400kHz（Fast Mode）  
> - 上升/下降时间与总线电容 Cb 相关：TR/TF = 20 + 0.1×Cb(pF) ns  
> - 总线电容上限 400pF

### 12.6 Reset Timing / 复位时序

*(PDF Page 44)*

**Figure reference: PDF Page 44, Reset Timing Diagram**

*(VDD1 = 1.65–3.5V, TA = +25°C)*

| Symbol | Parameter | Min | Typ | Max | Unit |
|--------|-----------|-----|-----|-----|------|
| tR | Reset time (internal reset duration) | — | — | 2.0 | ms |
| tRW | Reset LOW pulse width | 10.0 | — | — | μs |

*(VDD1 = 2.4–3.5V, TA = +25°C)*

| Symbol | Parameter | Min | Typ | Max | Unit |
|--------|-----------|-----|-----|-----|------|
| tR | Reset time | — | — | 1.0 | ms |
| tRW | Reset LOW pulse width | 5.0 | — | — | μs |

> **中文注释：**  
> - 复位脉冲宽度：VDD1=1.65–3.5V 时 ≥10μs；VDD1=2.4–3.5V 时 ≥5μs  
> - 内部复位完成时间：VDD1=1.65–3.5V 时 ≤2ms；VDD1=2.4–3.5V 时 ≤1ms  
> - 复位完成后才能发送初始化命令

---

## 13. Application Circuits / 典型应用电路

*(PDF Pages 45–48)*

> **中文注释：**  
> 以下应用电路均为参考设计（for reference only）。各接口模式的核心差异在于 IM0/IM1/IM2 配置、电源连接和不使用引脚的处理。

### 13.1 8080-Series Interface with Internal Oscillator and Built-in DC-DC

*(PDF Page 45, Figure 12)*

**Figure reference: PDF Page 45, Figure 12**

**External components:**
- C3–C5, C7: 4.7 μF
- C1, C2: 0.22 μF (charge pump capacitors)
- R1: ~510 kΩ (IREF resistor; R1 = (VIREF – VSS) / IREF)

**Connection notes:**
- IM0/IM1/IM2 set for 8080 mode
- CLS connected to VDD1 (internal oscillator)
- VDD2 connected to supply (built-in DC-DC)
- C1 (C1N/C1P) and C2 (C2N/C2P): charge pump capacitors connected

> **中文注释：**  
> 8080 并行接口 + 内部振荡器 + 内部 DC-DC 典型电路  
> R1 ≈ 510kΩ 用于设置 IREF 电流为 12.5mA  
> C1、C2（0.22μF）为电荷泵飞跨电容，C3–C5、C7（4.7μF）为电源去耦电容

### 13.2 6800-Series Interface with Internal Oscillator and Built-in DC-DC

*(PDF Page 46, Figure 13)*

**Figure reference: PDF Page 46, Figure 13**

**Notes:** Same external components as 8080 version. IM0/IM1/IM2 set for 6800 mode. MPU R/W connects to SH1106 WR; MPU E connects to SH1106 RD.

> **中文注释：**  
> 6800 接口将 MCU 的 R/W 接 SH1106 的 WR 引脚，E（Enable）接 RD 引脚。其余与 8080 电路相同。

### 13.3 SPI Interface (3-wire or 4-wire) with External Oscillator and External VPP

*(PDF Page 47, Figure 14)*

**Figure reference: PDF Page 47, Figure 14**

**External components:**
- C3–C5: 4.7 μF
- R1: ~510 kΩ

**Connection notes:**
- 3-wire SPI: IM0 fixed to VDD1
- 4-wire SPI: IM0 fixed to VSS
- A0 (D/C) not used in 3-wire SPI — fix to VSS
- CLS connected to VSS (external clock input)
- External clock connected to CL
- External VPP supply (max 14.0V) connected to VPP via capacitor C5
- VDD2: NC (not connected, as external VPP is used)
- C1N, C1P, C2N, C2P: not connected (charge pump not used)
- WR and RD: not used in SPI mode — fix to VSS or VDD1
- CS: can be fixed to VSS in SPI mode

> **中文注释：**  
> SPI 接口使用外部振荡器（CLS=低，时钟从 CL 引入）和外部 VPP 供电。  
> 使用外部 VPP 时：VDD2 断开，C1/C2 电容不接，C1N/C1P/C2N/C2P 浮空。  
> 外部 VPP 最大 14.0V。

### 13.4 I²C Interface with Internal Oscillator and Built-in DC-DC

*(PDF Page 48, Figure 15)*

**Figure reference: PDF Page 48, Figure 15**

**External components:**
- C3–C5, C7: 4.7 μF
- C1, C2: 0.22 μF (charge pump capacitors)
- R1: ~510 kΩ

**Connection notes:**
- D1 = SDA (serial data, bidirectional)
- D0 = SCL (serial clock input)
- SA0 (A0): slave address LSB — connect to VSS or VDD1
- D2–D7: fix to VSS (not used)
- WR, RD: fix to VSS or VDD1 (not used)
- CS: fix to VSS
- Pull-up resistors on SDA and SCL must be supplied by VDD1 (not a separate supply)
- Least significant bit of slave address set by SA0

> **中文注释：**  
> I²C 接口典型电路：D1=SDA，D0=SCL，SA0（即 A0 引脚）设定从机地址低位。  
> I²C 上拉电阻供电**必须使用 VDD1**，不可用其他电压。  
> D2–D7（SI 以外）均接 VSS，WR/RD 接 VSS 或 VDD1。

---

## 14. Revision History / 版本历史

*(PDF Page 49)*

| Version | Content | Date |
|---------|---------|------|
| 1.0 | Original release | Feb. 2012 |
| 2.0 | 1. Modified CS description in SPI mode. 2. Modified VDD2 to NC when external VPP used (Page 47). | Mar. 2012 |
| 2.1 | Modified maximum VPP voltage range to 14.0V. | Apr. 2012 |
| 2.2 | 1. VDD2 should be disconnected when VPP is supplied externally (Page 3). 2. CS description in SPI mode unified (Page 8). 3. E/ and RD/WR descriptions kept same in SPI and I²C (Page 8). 4. D2–D7 description unified when not used (Pages 8, 10, 11, 47, 48). 5. Modified data set of command D5H to 00H–FFH (Page 25). 6. Modified column address description to 131 (Page 19). | Apr. 2012 |
| 2.3 | Pages 32–34: Modified power on/off sequence. | Jun. 2013 |

---

*End of Extract / 提取结束*  
*For diagrams, timing waveforms, and pad location tables, refer to the original PDF: SH1106_V2.3.pdf*
