---
layout: post
title: Jekyll-compose插件
date: 2024-03-16 17:56 +0800
categories: [安装配置, Jekyll]
tags: [Jekyll]
---
> 使用ChatGPT翻译自[官方README.md](https://github.com/jekyll/jekyll-compose/blob/master/README.md)
{: .prompt-info}
## Jekyll::Compose
使用一些命令简化在 Jekyll 中的写作。

### 安装
将以下内容添加到您的应用程序的 Gemfile 中：

```ruby
gem 'jekyll-compose', group: [:jekyll_plugins]
```
然后执行：
```shell
bundle
```
### 用法
安装完毕后（见上文），运行 `bundle exec jekyll help` 应该会看到：

在帮助列表中，您将看到新命令可用：

```sh
draft      # 创建一个新草稿
post       # 创建一个新文章
publish    # 将草稿移动到 _posts 目录并设置日期
unpublish  # 将文章移回 _drafts 目录
page       # 创建一个新页面
rename     # 重命名一个文章并更新标题
compose    # 创建一个新文件
```
使用以下命令创建新页面：

```shell
bundle exec jekyll page "My New Page"
```
使用以下命令创建新文章：

```shell
bundle exec jekyll post "My New Post"
# 或指定 yaml 前置元数据中日期属性的自定义格式
bundle exec jekyll post "My New Post" --timestamp-format "%Y-%m-%d %H:%M:%S %z"
# 或使用 compose 命令
bundle exec jekyll compose "My New Post"
# 或使用 compose 命令并指定文章
bundle exec jekyll compose "My New Post" --post
# 或使用 compose 命令并指定 posts 集合
bundle exec jekyll compose "My New Post" --collection "posts"
```

使用以下命令创建新草稿：

```shell
bundle exec jekyll draft "My new draft"
# 或使用 compose 命令并指定草稿
bundle exec jekyll compose "My new draft" --draft
# 或使用 compose 命令并指定 drafts 集合
bundle exec jekyll compose "My new draft" --collection "drafts"
```

使用以下命令重命名草稿：

```shell
bundle exec jekyll rename _drafts/my-new-draft.md "My Renamed Draft"
# 或将其重新命名
bundle exec jekyll rename _drafts/my-renamed-draft.md "My new draft"
```

使用以下命令发布草稿：

```shell
bundle exec jekyll publish _drafts/my-new-draft.md
# 或指定发布日期
bundle exec jekyll publish _drafts/my-new-draft.md --date 2014-01-24
# 或指定 yaml 前置元数据中日期属性的自定义格式
bundle exec jekyll publish _drafts/my-new-draft.md --timestamp-format "%Y-%m-%d %H:%M:%S %z"
```

使用以下命令重命名文章：

```shell
bundle exec jekyll rename _posts/2014-01-24-my-new-draft.md "My New Post"
# 或指定特定日期
bundle exec jekyll rename _posts/2014-01-24-my-new-post.md "My Old Post" --date "2012-03-04"
# 或指定当前日期
bundle exec jekyll rename _posts/2012-03-04-my-old-post.md "My New Post" --now
```

使用以下命令取消发布文章：

```shell
bundle exec jekyll unpublish _posts/2014-01-24-my-new-draft.md
```

使用以下命令在集合中创建新文件：

```shell
bundle exec jekyll compose "My New Thing" --collection "things"
```

### 配置
要自定义默认插件配置，请编辑您的 jekyll 配置文件中的 jekyll_compose 部分。

自动在编辑器中打开新草稿或文章
```yaml
  jekyll_compose:
    auto_open: true
```
并确保您已设置 EDITOR、VISUAL 或 JEKYLL_EDITOR 环境变量。
例如，如果您希望在 Atom 编辑器中打开新创建的 Jekyll 文章和草稿，您可以在 shell 配置中添加以下行：

```sh
export JEKYLL_EDITOR=atom
```
JEKYLL_EDITOR 将覆盖默认的 EDITOR 或 VISUAL 值。
VISUAL 将覆盖默认的 EDITOR 值。

为草稿和文章设置默认前置元数据
如果您希望为新创建的文章或草稿添加默认前置元数据，您可以在 default_front_matter 配置键下指定您想要的任意数量，例如：

```yaml
jekyll_compose:
  default_front_matter:
    drafts:
      description:
      image:
      category:
      tags:
    posts:
      description:
      image:
      category:
      tags:
      published: false
      sitemap: false
```
下面这些也将自动添加：
- 在 date 属性下的创建时间戳。
- 在 title 属性下的标题属性
- 对于集合，您可以使用 default_front_matter 和集合名称作为配置键来为新创建的集合文件添加默认前置元数据，例如对于集合 things：

```yaml
jekyll_compose:
  default_front_matter:
    things:
      description:
      image:
      category:
      tags:
```
### 贡献
1. Fork 该项目（http://github.com/jekyll/jekyll-compose/fork）
2. 创建您的功能分支 (git checkout -b my-new-feature)
3. 运行测试和我们的代码检查 (script/cibuild)
4. 提交您的更改 (git commit -am 'Add some feature')
5. 推送到分支 (git push origin my-new-feature)
6. 创建新的 Pull Request

***基于现有提案提交 Pull Request***

当提交一个使用来自未合并的 pull request 的代码的 pull request 时，请注意以下事项：

- 拉取旧请求中提出的更改仍然是原作者的财产。 从他们离开的地方继续前进意味着您是共同作者。
- GitHub 允许将信用归属于多个作者。但是，该项目中的 pull request 会自动压缩然后合并到基本分支上。因此，只有作者和共同作者在合并后才会获得信用。
- 如果原始 pull request 包含多个提交，您可以将它们压缩成一个提交，但请确保列出任何额外的作者（和您自己）作为该提交的共同作者。
- 在您的 pull request 帖子中使用适当的关键字链接到现有的 pull request 或 issue-ticket，以便在合并您的 pull request 时它们会自动关闭。