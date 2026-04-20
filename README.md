# 我的笔记

> 使用 mdbook 生成, 通过 Github Pages 实现在线浏览。



页面主要是两个部分：

- 带密码的访问页 index.html
- mdbook生成的静态网页

Github Action 会建立一个临时目录 gh-page-files，会将 `带密码的访问页 index.html` 复制到这个目录下，然后使用 mdbook 编译输出到 gp-page-files/a 目录下，index.html 中就有一个需要点击的链接跳转到，a/index.html (这是mdbook编译出的静态网页的首页)。