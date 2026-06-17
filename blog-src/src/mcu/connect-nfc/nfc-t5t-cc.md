# 5 类卡的 CC 数据

`ST25DV16K` 遵循 `NFC Forum Type 5 Tag (T5T)` 规范



## 什么是 CC 数据

为了让 `NFC` 设备（如手机）能正确识别并读取标签上的NDEF消息，标签必须在**存储器起始位置**创建一个标准的 `Capability Container (CC)` 文件。这个文件相当于标签的 “自我介绍”，描述了其内存布局、支持的读写权限等关键信息。



## ST25DVXX 的CC数据

在每个**用户数据区**的第一个 Block 的开头，都会有一个CC数据。
根据不同的芯片型号，CC 数据的大小也不一样。

- 对于 `ST25DV04K` 和 `ST25DV04KC` ，CC 数据占用 4 字节

| 字节偏移 | 说明                           |
| -------- | ------------------------------ |
| 0        | Magic number                   |
| 1        | Version and access condition   |
| 2        | MLEN                           |
| 3        | Additional feature information |

- 对于 `ST25DV16K`、`ST25DV16KC`、`ST25DV64K`、 `ST25DV64KC`、`ST25TV16K`、`ST25TV64K`，CC 数据占用 8 字节。

| 字节偏移 | 说明                           |
| -------- | ------------------------------ |
| 0        | Magic number                   |
| 1        | Version and access condition   |
| 2        | MLEN                           |
| 3        | Additional feature information |
| 4-5      | RFU                            |
| 6-7      | MLEN                           |



**Magic number**

- 对于支持**单字节地址模式**的芯片，值是 `0xE1`
- 对于支持**双字节地址模式**的芯片，值是 `0xE2`
- 对于存储空间大于 2KB 的芯片，就必须用双字节地址模式



**Version and access condition**

- `bit[7:6]` ：主版本号，例如 `01b = Version 1.x`
- `bit[5:4]` ：次版本号，例如 `00b = Version x.0`
- `bit[3:2]` ：读取控制
  - `00b` = Always
  - `01b` = RFU
  - `10b` = Proprietary (专有的)
  - `11b` = REF
- `bit[1:0]` ：写入控制
  - `00b` = Always
  - `01b` = RFU
  - `10b` = Proprietary
  - `11b` = Never



**MLEN**

`MLEN (Encode Memory Length) `  表示  `T5T` 卡的 `NDEF Message` 的数据长度

实际的数据区域占用的空间大小= `MLEN * 8 (单位: 字节)`

例如在 `ST25DV04KC` 的芯片上，因为 CC 数据是 4 个字节，

- 那么如果用户数据区域都作为 `T5T` 数据区域，那 `MLEN = (512-4) / 8 = 63 = 0x3F`

- 如果只有一部分用户数据区域作为 `T5T` 数据区域的话，比如用了 128-Byte，那就是

  `MLEN = 128 / 8 = 16 = 0x10 Byte`

  对于更高容量的芯片计算方式也类似，只是需要考虑更大的CC数据

> 但上面的计算方式只适用于iOS系统以及Android Oreo版本或更高版本的系统，对于 Android Oreo 以下版本系统，需按用户数据区域的总大小去计算
>
> 例如小容量芯片，CC 数据是4字节的情况下，MLEN=512/8/64=0x40
>
> 对于大容量芯片，CC 数据是8字节的情况下，MLEN=8192/8=1024=0x400
>
> 这种计算方式不符合 NFC 规范，但是支持 iOS 与所有 Android 版本的正常使用



**Additional feature information**

`CC` 数据的第五个字节是其他功能信息，每个 bit 的作用如下：

- `bit[7:5]`  RFU
- `bit[4]`      是否支持专用框架
- `bit[3]`      是否锁定 Block（建议设置为 0）
- `bit[2:1]`  RFU（建议设置为0）
- `bit[0]`      是否支持读取多个 Block 命令





## 补充

 `CC` 数据区后面跟着的是 `T5T` 数据

在 `T5T` 数据中包含了 `NDEF Message TLV`，其中的 `TLV` 分别代表了 `Tag`，`Length`，`Value`

`NDEF Message TLV` 的格式如下

| 名称 | 长度   | 描述                                     |
| ---- | ------ | ---------------------------------------- |
| T    | 1byte  | 固定为 0x03，代表 `NDEF Message TLV`     |
| L    | 1byte  | `TLV` 长度                               |
| V    | N byte | `TLV` 值 （长度取决于上面的 TLV 长度值） |

`NDEF Message`的结束由一个 `Tag`字段为 `0xFE` 的 `TLV` 指示，该 `TLV` 被称为终止符 `TLV`





## 实例说明

下面是从 `ST25DV16K` 中读出的原始数据，我通过 I2C 接口写入了一个 URI : `http://git-scm.com`

```
E2 40 00 00 // 8字节CC数据
00 00 00 FF
03 10 D1 01 // T: 03, L: 10. 之后是一个 NEDF Record
0C 55 01 67 // 67: 'g'
69 74 2D 73 // 69: 'i', 74: 't'
63 6D 2E 63
6F 6D FE 00 // FE: 终止
00 00 00 00
```

