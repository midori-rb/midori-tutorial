# 路由

## 基本用法

用户应在继承自 `Midori::API` 的类中定义路由。  midori 不支持像 sinatra 那样定义全局路由，以此避免作用域污染，作用域污染将会对大规模的项目产生不可忽视的负面影响。

在 midori 中，路由是具有 URL 匹配模式的 HTTP 方法。 每个路由都与一个 block 相关联：

```ruby
class ExampleAPI < Midori::API
  get '/' do
    #.. show something ..
  end
  
  post '/' do
    #.. create something ..
  end
  
  put '/' do
    #.. replace something ..
  end
  
  delete '/' do
    #.. annihilate something ..
  end
  
  options '/' do
    #.. appease something ..
  end
  
  link '/' do
    #.. affiliate something ..
  end
  
  unlink '/' do
    #.. separate something ..
  end
end
```

路由按照定义的顺序进行匹配。第一个与请求相匹配的路由被调用。

midori 不仅支持上述方法，还支持 RFC 标准中的绝大部分方法。你可以查看 [API 文档](http://www.rubydoc.info/gems/em-midori/Midori/API) 来了解更多细节。

## Params

路由模式中可包含命名参数，可以通过 `request.params` 的 hash 值进行调用：

```ruby
class ExampleAPI < Midori::API
  get '/hello/:name' do
    "Ohayou #{request.params['name']}"
  end
end
```

路由模式中还可包含 splat （或通配符）参数，可以通过 `request.params['splat']` 数组进行调用：

```ruby
class ExampleAPI < Midori::API
  get '/say/*/to/*' do
    # matches /say/hello/to/world
    request.params['splat'] # => ["hello", "world"]
  end

  get '/download/*.*' do
    # matches /download/path/to/file.xml
    request.params['splat'] # => ["path/to/file", "xml"]
  end
end
```

路由也可使用 query string ：

```ruby
class ExampleAPI < Midori::API
  get '/posts' do
    # matches "GET /posts?title=foo&author=bar"
    request.query_string # => title=foo&author=bar
  end
end
```

## WebSocket 和 EventSource


`WebSocket` 连接使用 HTTP 协议中的 `GET` 方法，但实际上它与 `GET` 请求完全不同。用户无需关心协议细节。在 midori 中可以轻松地轻松管理 websocket 连接。

这里提供一个聊天室示例，来演示如何在 midori 中使用 websocket：

```ruby
CONNECTION_POOL = []

class ExampleAPI < Midori::API
  websocket '/' do |ws|
    ws.on :open do
      ws.send 'Ohayo Midori'
      CONNECTION_POOL << ws
    end
    
    ws.on :message do |msg|
      CONNECTION_POOL.map do |client|
        client.send msg
      end
    end
    
    ws.on :close do
      CONNECTION_POOL.delete(ws)
      puts 'Oyasumi midori'
    end
  end
end
```

midori 也支持将 `EventSource` 连接加入到你的路由中。

这里提供一个聊天室示例，来演示如何在 midori 中使用 eventsource ：

```ruby
CONNECTION_POOL = []

class ExampleAPI < Midori::API
  post '/pub' do
    clients = CONNECTION_POOL.clone
    CONNECTION_POOL.clear
    # EventSource connection disconnects every time message sent, DO NOT reuse connection pool
    clients.map do |client|
      client.send request.body
    end
  end
  
  eventsource '/sub' do |es|
    CONNECTION_POOL << es
  end
end
```

