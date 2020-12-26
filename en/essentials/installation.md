# Installation

## Requirements

Open up a command line prompt. Any commands prefaced with a dollar sign `$` should be run in the command line. Verify that you have a current version of Ruby installed:

```
 $ ruby -v
 ruby 2.5.1p57
```

Before every version of midori released, it would be tested and **officially ensured** in several ruby versions, which would be noticed in the release news post. In general, it would be tested in latest stable versions of all **still under maintenance**. For now, the latest midori supports the following ruby interpreters:

- Ruby (MRI)
  - 2.3.7
  - 2.4.5
  - 2.5.1

**Note:**

- **For JRuby users, due to some C extensions used in midori, it is still unable to run on the current version of JRuby.**
- **For macOS users, you may meet performance problem due to the issue of [nio4r](https://github.com/socketry/nio4r/issues/125). As very few people would use macOS in production, this issue may not affect much. We would still be working hard on fixing it, but the issue wouldn't be a high priority one.**

It's hard to say that if it is possible for running on other ruby implementations like Rubinius or RubyMotion, if you're in favor of supporting more ruby implementations, you could open a ticket [here](https://github.com/midori-rb/midori.rb/issues), and we are glad to discuss it.

## Install with RubyGems

```
$ gem install midori.rb
Successfully installed midori.rb-0.8.0
1 gem installed
```

## Use Bundler

Example `Gemfile` of basic usage as following:

```ruby
source 'https://rubygems.org'
gem 'bundler', '~> 1.0'
gem 'midori.rb', '~> 0.8'
```

and then running:

```
$ bundle install
```

You could use

```ruby
require 'bundler'
Bundler.require
```

in your entrance ruby file.

To include built-in extensions of midori you could make your `Gemfile` like:

```ruby
source 'https://rubygems.org'
gem 'bundler', '~> 1.0'
gem 'midori.rb', '~> 0.8'
gem 'midori-contrib', '~> 0.0.1', require: %w'midori-contrib/file midori-contrib/sequel/mysql2'
```

Using bunlder could make dependency management much easier, which helps a lot in scaling project. To learn more about bundler, you could see docs [here](http://bundler.io/docs.html). 

## For Developers in China

You may probably meet problems with rubygems due to unstable overseas internet connection issues in China. The most popular way to solve it is to use mirror provided by [RubyChina](https://gems.ruby-china.com/) or [TUNA](https://mirror.tuna.tsinghua.edu.cn/help/rubygems/) as your gem source. This may have some side effects in development, because there's a few minutes' delay in receiving gem updates.

Alternatively, you could use proxy to connect to the main repository directly to avoid the delay problem. But using proxy is a little too complex in production environment.

Choose the solution better fits your requirements. Mixing the solutions like using proxy in development and using mirror in production is also a good choice.
