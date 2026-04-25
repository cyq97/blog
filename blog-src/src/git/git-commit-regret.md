# Git 提交出错了? 后悔了?



> [!IMPORTANT]
>
> 以下的命令**只要没 git push 到远端**
>
> 你怎么改、怎么删、怎么撤销 commit **都完全安全**，不会影响别人





## 修改上一次 commit message

- 执行后，最后一次提交的信息就被改掉了，代码不变。

```shell
git commit --amend -m "新的commit message"
```



## 撤销 commit

- **撤销上一次 commit**

- 代码文件还在，**变回 git add 之前的状态**

- 可以重新修改、重新提交

```shell
git reset --mixed HEAD^
```



## 撤销最后 N 次 commit

- 可以多次执行 `git reset --mixed HEAD^` 来持续往前撤销
- 也可以直接指定往前撤销指定次数的提交

```shell
# 撤销最后 2 次提交
git reset --mixed HEAD~2

# 撤销最后 5 次提交
git reset --mixed HEAD~5
```



## 撤销到某个历史版本

- 可以直接撤销到某一个历史版本

```shell
git reset --mixed 历史版本的哈希值
```





## 彻底撤销最后一次 commit

> [!WARNING]
>
> ⚠️**不保留代码，慎用**
>
> ⚠️ 会直接把**代码恢复到上上个版本**，当前提交的**改动会丢失**，本地没推也建议先备份再用。

```shell
git reset --hard HEAD^
```









