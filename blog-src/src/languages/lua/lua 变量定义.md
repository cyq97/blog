# lua 变量定义

> [lua变量定义 lua快速入门](https://www.fengfengzhidao.com/special/8/89)



Lua声明变量是不需要声明类型的，并且声明之后后期也可以修改类型，这一点和python是类似的

```lua
--这是注释

--直接定义的是全局变量

a = 1
b = "fengfeng"
c = 'zhidao' -- 单引号也可以
d = true
e = false
f = { 1, 2, 3 } -- 数组
g = { name = "fengfeng", age = a } -- 表

--也可以这样批量赋值
h, i, j = "a", 2, false


--使用print打印，type显示类型
print("a", a, type(a))
print("b", b, type(b))
print("c", c, type(c))
print("d", d, type(d))
print("e", e, type(e))
print("f", f, type(f))
print("g", g, type(g))
print("h i j", h, i, j)

--动态改类型也是可以的
a = "zhangsan"
print("a", a, type(a))
```



## 局部变量

使用 `local` 关键字声明局部变量，局部变量的作用范围是当前的代码块

```lua
do
    --这是代码块
    local a = 1
    print("a", a)
end
print("a", a) -- 打印为nil
```



## 数组

Lua 无原生数组，通过 **table + 数字索引** 模拟，默认索引从 1 开始

`#` 符号的局限性：仅对 “连续数字索引” 有效，非连续数组需手动计算长度

```lua
local array = {"A", nil, "B", "C", "D", 1, 2, 3, 4}
print(array, type(array)) -- table
print(array[1]) -- A
print(array[-1]) -- 访问不存在的索引，nil

-- 获取数组长度
print(#array)
print(array[#array]) -- 4

--遍历数组
-- 方式1：按索引遍历（推荐，可控）
for i = 1, #array do
    print(i, array[i])
end

-- 方式2：用 pairs 遍历（会遍历所有键值对，包括非连续索引） 遇到nil 跳过
for k, v in pairs(array) do
    print("pairs 键：" .. k .. "，值：" .. v)
end

-- 方式3：用 ipairs 遍历（只遍历连续的数字索引，从 1 开始） 遇到nil break
for k, v in ipairs(array) do
    print("ipairs 键：" .. k .. "，值：" .. v)
end
```



## 表

这是 Lua 最核心、最强大的数据结构 ——Lua 里几乎所有复杂数据类型（数组、字典、对象等）都是基于 table 实现的

table 是一种**键值对（key-value）** 集合，类似 Python 的字典（dict），但比字典更灵活：

- 键（key）可以是**除 nil 外的任意类型**（数字、字符串、布尔、甚至另一个 table）；
- 值（value）可以是任意类型（数字、字符串、函数、table 等）；
- table 是**动态的**，可随时添加 / 删除键值对；
- table 是**引用类型**（赋值、传参时传递的是地址，而非拷贝）。

```lua
-- 方式1：空 table
local t1 = {}

-- 方式2：初始化键值对（推荐）
-- ① 隐式键（数字索引，默认从 1 开始，对应之前讲的数组）
local t2 = {"Lua", "Python", "Java"}  -- 等价于 t2[1]="Lua", t2[2]="Python", t2[3]="Java"

-- ② 显式键（任意类型）
local t3 = {
    name = "张三",       -- 等价于 t3["name"] = "张三"（字符串键）
    age = 20,            -- 数字值
    is_student = true,   -- 布尔值
    [100] = "分数",      -- 数字键（显式写法）
    ["hobby-list"] = {"读书", "运动"}  -- 字符串键（含特殊字符）+ 值为 table
}

-- ③ 混合键（数组 + 字典）
local t4 = {
    10, 20, 30,          -- 隐式数字键：1、2、3
    name = "李四",       -- 显式字符串键
    [false] = "布尔键"   -- 显式布尔键
}
```

### 访问table元素

```lua
local t = {name = "张三", age = 20, [1] = "hello"}

-- 访问方式1：点语法（仅适用于 字符串键且符合标识符规则，如字母/数字/下划线）
print(t.name)   -- 输出：张三
print(t.age)    -- 输出：20
-- print(t.1)    -- 报错！点语法不能用于数字键

-- 访问方式2：方括号语法（通用，适用于所有键类型）
print(t["name"])  -- 输出：张三
print(t[1])       -- 输出：hello
print(t["age"])   -- 输出：20

-- 访问不存在的键，返回 nil
print(t.gender)   -- 输出：nil
```

### 修改table元素

```lua
local t = {name = "张三"}

-- 添加新键值对
t.age = 20                -- 字符串键
t[100] = "满分"           -- 数字键
t["hobby"] = "跑步"       -- 字符串键（显式）

-- 修改已有键值对
t.name = "李四"
print(t.name)  -- 输出：李四

-- 删除键值对（赋值为 nil）
t.age = nil
print(t.age)   -- 输出：nil
```

注意事项

1. **键的隐式转换**：数字键和字符串键是不同的（如 `t[1]` ≠ `t["1"]`）；
2. **# 符号的局限性**：仅对连续数字索引有效，字典型 table 别用 `#` 求长度；
3. **nil 的影响**：`ipairs()` 遇到 nil 会停止遍历，如需 “空元素” 可用 0 / 空字符串占位；
4. **引用赋值**：避免误以为 `t2 = t1` 是拷贝，修改 t2 会影响 t1。

