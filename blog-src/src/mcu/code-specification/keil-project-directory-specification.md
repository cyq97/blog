# MDK5-KEIL代码工程的目录规范



```shell
code-Repository
├── docs                            # 仓库文档
├── libs                            # 子模块组成的库
|   └── submodule_01                # 子模块
├── projects                        # 存放多个MDK-KEIL工程
|   └── prj_01                      # 某个MDK-KEIL工程
|       ├── mdk-arm                 # 当前MDK-KEIL工程目录
|       |   ├── scripts             # 当前MDK-KEIL工程编译时使用的脚本
|       |   |   └── target_01       # MDK-KEIL工程可以有多个target, 每个target可以有各自的脚本
|       |   └── build               # 当前MDK-KEIL工程编译输出目录
|       |       └── target_01       # MDK-KEIL工程可以有多个target, 每个target有各自的输出
|       |           ├── objects     # MDK-KEIL工程的Objects文件夹
|       |           └── listings    # MDK-KEIL工程的Listings文件夹
|       ├── src                     # 当前工程特定源码
|       |   ├── main                # 存放主函数等
|       |   └── mcu-device          # 存放芯片启动文件等 cmsis 标准文件和厂商提供的芯片配置程序
|       ├── link                    # 存放链接脚本, MDK-KEIL中称为分散加载文件
|       └── docs                    # 当前工程的文档
├── tools                           # 工具软件
└── README.md                       # 仓库自述文档
```

<br>



**仓库根目录**

- docs

- libs

  *诸如 MCU SDK 这类，都是以子模块的仓库进行管理的，存放在此目录中*

- projects

- tools

- README.md



**特定工程目录**

- mdk-arm

- src

  *工程特点的源码, 比如芯片的cmsis文件(启动文件，时钟配置), 主函数文件*

- link

  *将链接脚本直接存放到工程目录而不是 mdk-arm 目录，是为了方便在不同开发环境下能同一观察到链接配置，毕竟更改开发环境时各种数据的存放方式和位置我们还是不想变动的。*

  *推荐直接将链接脚本的名字命名为 target 的名字(+文件拓展名)。*

  *很多情况下一般只有一个 target，推荐此时将这个 target 命名为 **target_default***

- docs



**mdk-arm 目录**

- scripts

  keil 的 "Options for target"(魔术棒) -> "User" 里面支持添加编译前编译后自动执行脚本, 此目录就用来存放这些文件, 比如常用的Hex转bin的处理

- build
