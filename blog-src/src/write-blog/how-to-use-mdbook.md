# 如何使用 mdbook



>  官方文档：[mdBook Documentation](https://rust-lang.github.io/mdBook/)  
>
> `https://rust-lang.github.io/mdBook/`





## 配置文件

`book.toml`

```toml
# 设置 mdbook build 的输出路径
[build]
build-dir = "specific-output-path"
```



## 自动编译文档

打开两个终端：

- 第一个运行 `mdbook serve`，此命令会监控 `src` 目录中的变化，每次发生变化时都会重新构建。
- 第二个运行 `mdbook watch`，此命令监控文件，并在修改任何文件时自动触发构建。



## 文档结构

> 🏆🏆🏆 参见官方文档 format - summary 章节

官方文档的 `SUMMARY.md` 的内容如下，这展示了一个标准的文档结构。

```markdown
# Summary

[Introduction](README.md)

# User guide

- [Installation](guide/installation.md)
- [Reading books](guide/reading.md)
- [Creating a book](guide/creating.md)

# Reference guide

- [Command-line tool](cli/README.md)
    - [init](cli/init.md)
    - [build](cli/build.md)
    - [watch](cli/watch.md)
    - [serve](cli/serve.md)
    - [test](cli/test.md)
    - [clean](cli/clean.md)
    - [completions](cli/completions.md)
- [Format](format/README.md)
    - [SUMMARY.md](format/summary.md)
        - [Draft chapter]()
    - [Configuration](format/configuration/README.md)
        - [General](format/configuration/general.md)
        - [Preprocessors](format/configuration/preprocessors.md)
        - [Renderers](format/configuration/renderers.md)
        - [Environment variables](format/configuration/environment-variables.md)
    - [Theme](format/theme/README.md)
        - [index.hbs](format/theme/index-hbs.md)
        - [Syntax highlighting](format/theme/syntax-highlighting.md)
        - [Editor](format/theme/editor.md)
    - [MathJax support](format/mathjax.md)
    - [mdBook-specific features](format/mdbook.md)
    - [Markdown](format/markdown.md)
- [Continuous integration](continuous-integration.md)
- [For developers](for_developers/README.md)
    - [Preprocessors](for_developers/preprocessors.md)
    - [Alternative backends](for_developers/backends.md)

-----------

[Contributors](misc/contributors.md)

```



1. **标题 - Title**

   这是可选的，但文档通常以标题开头，一般为 `# Summary`。不过解析器会忽略它，也可以省略。

<br>



2. **前置章节 - Prefix Chapter**

   如上面示例中的 `[Introduction](README.md)`。

   **在主编号章节之前，可以添加不进行编号的前置章节**。这适用于前言、引言等内容。

   不过存在一些限制。前置章节**不能嵌套**，必须**全部位于根层级**。并且**一旦添加了编号章节，就无法再添加前置章节**。

<br>



3. **部分标题 - Part Title**

   如上面示例中的 `# User guide`、`# Reference guide`。

   一级标题（一个`#`）可用作后续带编号章节的标题。

   这可用于**在逻辑上分隔书籍的不同部分**。

   标题会显示为**不可点击的文本**。标题为可选内容，带编号的章节可根据需要拆分为多个部分。

   部分标题**必须是一级标题（一个`#`）**，其他标题级别将被忽略。

<br>



4. **编号章节 - Numbered Chapter**

   如上面示例中存在诸如 `- [xxxxx](yyyyy.md)` 这种格式的条目。

   编号章节概述了书的主要内容，并且可以嵌套，从而形成一个很好的层次结构（章节、子章节等）。

<br>



5. **后缀章节 - Suffix Chapter**

   如上面示例中最后的 `[Contributors](misc/contributors.md)`。

   与前缀章节类似，后缀章节也没有编号，但它们位于带编号的章节之后。

<br>



6. **草稿章节 - Draft chapters**

   如上面示例中的 `- [Draft chapter]()`

   草稿章节是指**没有对应文件及内容**的章节。

   设置草稿章节的目的是标记后续待撰写的章节。也可在规划书籍结构时使用，避免在频繁调整书籍结构的过程中反复创建文件。

   草稿章节在 HTML 渲染器的目录中会显示为禁用链接。

   草稿章节的**编写方式与普通章节一致，只需不填写文件路径即可**。

<br>



7. **分隔符 - Separators**

   如上面示例中的 `-----------`

   分隔符可添加在任意其他元素之前、之间和之后。

   它们会在生成的目录的 HTML 渲染行中体现。

   分隔符是指仅由短横线组成且至少包含三个短横线的行：`---`。
