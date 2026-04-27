# AT32F403A 引脚笔记



## I2C

| 引脚 | I2C功能       | 冲突                                                         |
| ---- | ------------- | ------------------------------------------------------------ |
| PB6  | **I2C1_SCL**  | SPIM_IO3<br />TMR4_CH1<br />USART1_TX<br />I2S1_MCK<br />SPI4_CS<br />I2S4_WS |
| PB8  | **I2C1_SCL**  | SDIO1_D4<br />TMR4_CH3<br />TMR10_CH1<br />UART5_RX<br />SPI4_MISO<br />CAN1_RX |
| PB7  | **I2C1_SDA**  | XMC_NADV<br />SPIM_IO2<br />TMR4_CH2<br />USART1_RX<br />SPI4_SCK<br />I2S4_CK |
| PB9  | **I2C1_SDA**  | SDIO1_D5<br />TMR4_CH4<br />TMR11_CH1<br />UART5_TX<br />SPI4_MOSI<br />I2S4_SD<br />CAN1_TX |
| PB5  | **I2C1_SMBA** | SPI3_MOSI<br />I2S3_SD<br />SPI1_MOSI<br />I2S1_SD<br />CAN2_RX<br />TMR3_CH2 |
|      |               |                                                              |
| PB10 | **I2C2_SCL**  | USART3_TX<br />I2S3_MCK<br />SPIM_IO0<br />TMR2_CH3          |
| PB11 | **I2C2_SDA**  | USART3_RX<br />SPIM_IO1<br />TMR2_CH4                        |
| PB12 | **I2C2_SMBA** | USART3_CK<br />CAN2_RX<br />SPI2_CS<br />I2S2_WS<br />TMR1_BRK<br />XMC_D13 |
|      |               |                                                              |
| PA8  | **I2C3_SCL**  | CLKOUT<br />USART1_CK<br />USBFS_SOF<br />SPIM_CS<br />TMR1_CH1 |
| PB4  | **I2C3_SDA**  | NJTRST<br />SPI3_MISO<br />I2S3_SDEXT<br />SPI1_MISO<br />UART7_TX<br />TMR3_CH1 |
| PC9  | **I2C3_SDA**  | SDIO1_D1<br />TMR8_CH4<br />TMR3_CH4                         |
| PA9  | **I2C3_SMBA** | USART1_TX<br />TMR1_CH2                                      |

