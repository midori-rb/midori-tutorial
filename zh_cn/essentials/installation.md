# 安装

## 环境需求

打开命令行提示符，所有以 `$` 符号开头的命令都应该在命令行中运行。

确认你安装了当前版本的 Ruby：


```
 $ ruby -v
 ruby 2.5.1p57
```

每个版本的 midori 发布之前，会在几个 ruby 版本中进行测试，做到**官方保证其能正确运行**，你可以在更新说明中看到这些内容。一般来说，我们将在所有**仍然在维护的最新稳定版本中进行测试**。目前，最新的 midori 支持以下 ruby 解释器：

- Ruby (MRI)
  - 2.3.7
  - 2.4.5
  - 2.5.1


**注意：**

- **对于 JRuby 用户，由于 midori 使用了一些 C 扩展，在当前版本的 JRuby 上还无法运行。**
- **对于 macOS 用户，你也许会遇到一些由于 [nio4r](https://github.com/socketry/nio4r/issues/125) 导致的性能问题。 由于在 macOS 上使用产品的用户较少，所以这个问题应该影响范围较小。 我们仍会继续努力修复它， 但不会将其放在首要位置。**

我们不确定是否能在其他 ruby 实现，如 Rubinius 或 RubyMotion 上，安装并使用 midori 。如果你对支持更多 ruby 实现有兴趣，可以在[这里](https://github.com/midori-rb/midori.rb/issues)创建 ticket， 我们愿意就此进行讨论。

## 通过 RubyGems 安装

```
$ gem install em-midori
Successfully installed midori.rb-0.8.0
1 gem installed
```

测试是否成功安装，运行以下命令:

```
$ ruby -r "midori" -e "class A < Midori::API;end;Midori::Runner.new(A).start"
```

如果你看到以下信息，表示一切正常运行。

```
Midori 0.4.3 is now running on 127.0.0.1:8080
```

## 使用 Bundler

示例 `Gemfile` 的基本用法如下:

```ruby
source 'https://rubygems.org'
gem 'bundler', '~> 1.0'
gem 'midori.rb', '~> 0.8'
```

之后运行:

```
$ bundle install
```

你可以在 ruby 入口文件中使用：

```ruby
require 'bundler'
Bundler.require
```

如要使用 midori 内建的扩展，你可以这样写 `Gemfile`:

```ruby
source 'https://rubygems.org'
gem 'bundler', '~> 1.0'
gem 'midori.rb', '~> 0.8'
gem 'midori-contrib', '~> 0.0.1', require: %w'midori-contrib/file midori-contrib/sequel/mysql2'
```

使用 Bunlder 可以使依赖管理更容易， 提高了项目的可扩展性。想了解更多关于 Bunlder 的内容， 你可以查看[这里](http://bundler.io/docs.html)的文档。 

## 对于中国开发者

由于中国大陆对外网络访问不稳定，你可能会遇到 rubygems 的相关问题。

最常用的解决方法是使用 [RubyChina](https://gems.ruby-china.com/) 或 [TUNA](https://mirror.tuna.tsinghua.edu.cn/help/rubygems/) 提供的镜像作为你的 gem 源。这样可能会对开发产生一些影响，因为在获取 gem 更新时会有数分钟的延迟。

另一种方式是通过代理服务器直接连接主仓库来避免延迟问题。 但在产品中使用代理服务器还是有一点过于复杂。

根据需求选择合适的方法解决你的问题。将不同方法结合起来，如在生产环境中使用代理服务器，在产品中使用镜像源也是一种好方法。

