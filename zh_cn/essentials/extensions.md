# 扩展

扩展是一类 rubygems，可以通过元编程将其他 gems 与 midori 整合。

官方提供了一套名为 `midori-contrib` 的 midori 扩展，其中包含了常用的 gems， 如  Database ORM、Redis Driver、Redis ORM、HTTP Driver 等。

向你的项目中加入扩展时，请先确保已安装了原版的 gem 且它没有被调用。然后引入 `midori-contrib` 的特征部分使其生效。

示例：

`Gemfile`

```ruby
source 'https://rubygems.org'

gem 'bundler', '~> 1.0'
gem 'rake', '~> 12.0'

gem 'hiredis', '~> 0.6.0', require: false
gem 'ohm', '~> 3.0'
gem 'sequel', '~> 5.0', require: false
gem 'mysql2', '~> 0.4', require: false

gem 'em-midori', '~> 0.4.3', require: 'midori'
gem 'midori-contrib', '~> 0.1.0', require: %w(
    midori-contrib
    midori-contrib/sequel/mysql2
    midori-contrib/redic
  )
```
