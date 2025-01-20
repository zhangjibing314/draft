# github pages
## 简介
[github pages][github pages] github 上的一个静态站点托管服务
## github pages 站点的类型
类型有 3 中：项目、用户、组织
* 项目：
    * 作用：关联 github 上的指定项目
    * 解释: 在 github 的指定项目中，设置pages 选项即可完成设置，置于访问，似乎需要向完成 ‘用户’类型的站点才行(这里还不是很明白)
* 用户：
    * 作用:关联到 github 上的特定账户
    * 解释:需要创建一个名为"accounter_name.github.io" 的仓库, 然后通过 url “accounter_name.github.io” 就能访问，如果设置好了这个用户下面的其他项目，则可以通过 url "acounter_name.github.io/project_name" 来访问
* 组织：关联到 github 上的特定账户
## 使用 jekyll 设置站点
* jekyll 是一个静态站点生成器，内置 github pages 支持和简化的构建过程

github pages 中可以通过 _config.yml 文件来配置大多数 jekyll 设置

[github pages]: https://docs.github.com/zh/pages
