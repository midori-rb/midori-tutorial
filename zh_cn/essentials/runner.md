# 执行器

## 简介

`Runner` 是 Midori 服务器的容器。你可以通过 `Runner` 创建、启动、停止 midori 实例。

`Runner` 用 `Midori::Configure` 作为默认配置。

## 范例

这里有一些常用的例子。

### 端口绑定

使用端口 `4567` 启动 midori 实例，而不是默认的 `8080`。

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

启动 midori 实例监听所有 IP 地址。

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

### 停止 Midori

当指定的路由被调用时停止 midori 实例。

```ruby
require 'midori'
$runner = nil
class API < Midori::API
  get '/stop' do
    $runner.stop
  end
end

$runner = Midori::Runner.new(API).start
```
