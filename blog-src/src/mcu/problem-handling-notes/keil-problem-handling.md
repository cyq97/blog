# Keil 使用相关问题处理







## 1. 同名源文件导致的烦人的提示

**😕 问题描述**

在使用 keil 的时候，有时候会不小心添加了两个相同的 .c 文件，会导致编译出现如下的提示：

```
Note: object file renamed from "xxx.o" to "xxx_1.o"
```

即便删除多余的 .c 文件也不能消除提示。



**😀 处理方法**

1. 删除多余的 .c 文件

2. 在剩下的有效的 xxx.c 文件（或者文件所在的虚拟文件夹）上点击右键

   选择 `Options for File 'xxx.c' …`

   取消 `include in Target Build` 处的勾选, 点击 `OK` 后，重新编译工程

3. 点击顶部菜单栏 `Project` → `Clean Targets` 清理工程

4. 再次打开第 2 步的选项配置

   勾选 `include in Target Build`，点击 `OK` 后，重新编译工程
