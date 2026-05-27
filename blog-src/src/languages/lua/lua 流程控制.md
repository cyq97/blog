# lua 流程控制

> [lua流程控制 lua快速入门](https://www.fengfengzhidao.com/special/8/90)



## 条件判断

Lua 的 if 语句用于根据条件执行不同代码块，核心语法和其他语言类似，但**没有 elif 关键字**，需用 `elseif`（连写）替代，且结束必须用 `end`。

```lua
-- 单分支：只有 if
local score = 85
if score >= 60 then
    print("及格")
end

-- 双分支：if + else
if score >= 90 then
    print("优秀")
else
    print("非优秀")
end

-- 多分支：if + elseif + else（注意是 elseif，不是 elif/else if）
if score >= 90 then
    print("优秀")
elseif score >= 80 then
    print("良好")
elseif score >= 60 then
    print("及格")
else
    print("不及格")
end
```



## 循环语句

Lua 提供 3 种核心循环：`while`、`for`（数值 for / 泛型 for）、`repeat...until`，覆盖所有循环场景。

### while循环

语法：`while 条件 do ... end`，满足条件时执行循环体，适合循环次数不确定的场景。

```lua
-- 示例：计算 1-10 的和
local sum = 0
local i = 1
while i <= 10 do
    sum = sum + i
    i = i + 1  -- 必须手动更新循环变量，否则死循环
end
print(sum)  -- 输出：55
```

### for循环

Lua 的 for 分两种：**数值 for**（遍历数字范围）、**泛型 for**（遍历集合，如 table）。

数值 for（核心格式：for 变量=起始值, 结束值, 步长 do ... end）

```lua
-- 示例1：默认步长 1，遍历 1-5
for i = 1, 5 do
    print(i)  -- 输出：1 2 3 4 5
end

-- 示例2：指定步长 2，遍历 1-10 的奇数
for i = 1, 10, 2 do
    print(i)  -- 输出：1 3 5 7 9
end

-- 示例3：步长为负数，倒序遍历
for i = 5, 1, -1 do
    print(i)  -- 输出：5 4 3 2 1
end
```

泛型 for（遍历集合，结合 `ipairs/pairs`）

```lua
-- 示例1：遍历数组型 table（ipairs：连续数字索引）
local arr = {"Lua", "Python", "Java"}
for k, v in ipairs(arr) do
    print("索引：", k, "值：", v)  -- 输出：1 Lua / 2 Python / 3 Java
end

-- 示例2：遍历字典型 table（pairs：所有键值对）
local dict = {name = "张三", age = 20}
for k, v in pairs(dict) do
    print("键：", k, "值：", v)  -- 输出：name 张三 / age 20（顺序不固定）
end
```



repeat...until 循环（直到型循环）

语法：`repeat ... until 条件`，先执行循环体，再判断条件（**至少执行一次**），适合 “先做后判断” 的场景。

```lua
-- 示例：输入数字，直到输入大于 100 为止
local num = 0
repeat
    print("请输入一个数字：")
    num = tonumber(io.read())  -- 读取控制台输入并转为数字
until num > 100  -- 条件满足时退出循环
print("你输入了大于 100 的数字：", num)
```



## 流程跳转

用于中断或跳转代码执行，Lua 支持 `break`、`return`、`goto`（标签跳转）。

- 不等于用~=
- lua没有continue
- 慎用goto

```lua
-- 示例：用 goto 实现循环
local i = 1
::loop::  -- 定义标签
if i > 5 then
    goto end_loop  -- 跳转到结束标签
end
print(i)
i = i + 1
goto loop  -- 跳回循环标签
::end_loop::  -- 结束标签
print("循环结束")
```

