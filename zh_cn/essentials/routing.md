# 路由

## 基本用法

定义一个类继承  `Midori::API`  类即可完成路由的定义。Midori 不支持像 Sinatra 那样全局定义路由以避免作用域污染，这大大提高了复杂项目的可维护性。

在 midori 中，路由包含一个 HTTP 方法和 URL 的模式匹配。每个路由都关联一个 block：

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

路由按其定义的顺序进行匹配。第一个匹配上请求的路由将被调用。

Midori 不仅支持上述方法，它还支持 RFC 标准中提供的几乎所有方法。你可以在 [API 文档](http://www.rubydoc.info/gems/em-midori/Midori/API) 中查看更多细节。

## Params

路由模式可以包含命名参数，通过访问  `request.params`  获得：

```ruby
class ExampleAPI < Midori::API
  get '/hello/:name' do
    "Ohayou #{request.params['name']}"
  end
end
```

路由模式可以包含通配符，通过访问  `request.params['splat']`  获得：

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

路由也可以使用 query string：

```ruby
class ExampleAPI < Midori::API
  get '/posts' do
    # matches "GET /posts?title=foo&author=bar"
    request.query_string # => title=foo&author=bar
  end
end
```

## WebSocket 和 EventSource

`WebSocket`  连接在 HTTP 协议中使用  `GET`  方法。但事实上，它与  `GET`  请求的行为完全不同。 在 midori 中，您可以轻松管理 WebSocket 连接，而无需关心协议的细节。

下面是一个在 midori 中使用了 WebSocket 的聊天室示例：

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

midori 还支持在路由中定义  `EventSource`。

下面是一个在 midori 中 使用 EventSource 的聊天室示例：

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

