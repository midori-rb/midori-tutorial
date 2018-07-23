# 执行器

## 介绍

`Runner` 是 midori 服务器的容器。你可以通过 `Runner` 创建、启动或终止 midori 示例。 

`Runner` 使用 `Midori::Configure` 作为默认配置。

## 示例

这里给出几种基本用法的示例。

### 端口绑定

启动使用 `4567` 端口而非默认的 `8080` 端口的 midori 实例。

```ruby
require 'midori'
class API < Midori::API
  get '/' do
    'Hello World'
  end
end

Midori::Configure.set :port, 4567
Midori::Runner.new(API).start
```

### 地址绑定

启动监听所有 IP 地址的 midori 实例。

```ruby
require 'midori'
class API < Midori::API
  get '/' do
    'Hello World'
  end
end

Midori::Configure.set :bind, '0.0.0.0'
Midori::Runner.new(API).start
```

### 终止 midori

调用指定路由时终止 midori 实例。

```ruby
require 'midori'
$runner = nil
class API < Midori::API
  get '/stop' do
    $runner.stop
    nil
  end
end

$runner = Midori::Runner.new(API)
$runner.start
```
