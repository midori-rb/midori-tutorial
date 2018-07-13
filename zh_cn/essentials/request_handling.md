# 请求处理

## 访问请求对象

通过 `request` 方法，可以从不同的请求级别(filter、routes、error handlers)访问传入的请求对象：

```ruby
class ExampleAPI < Midori::API
  get '/' do
    request.ip           # '127.0.0.1' client ip address
    request.port         # '8080' client port
    request.method       # 'GET'
    request.path         # '/'
    request.query_string # ''
    request.query_params # {} query params, parsed from query string
    request.header       # {} request header
    request.body         # request body sent by the client
    request.params       # {} params matched in router
    request.cookie       # {} cookie parsed from header
  end
end
```

**(sinatra 用户)注意:** midori 中的 `request.body` 是 `String` 对象，而不是 `StringIO` 对象。

## 构建响应对象

默认情况下， midori 接收 block 的返回值作为响应体。

你可以通过编辑 `status` 和 `header` 变量来构建响应体之外的其他部分。

你也可以返回一个 `Midori::Response` 对象作为响应，这将会覆盖其它所有响应。

```ruby
class ExampleAPI < Midori::API
  get '/case_0' do
    'Hello'
    # HTTP/1.1 200 OK
    # Server: Midori/1.0
    #
    # Hello
  end
  
  get '/case_1' do
    @status = 418
    "I\'m a teapot"
    # HTTP/1.1 418 I'm a teapot
    # Server: Midori/1.0
    #
    # I'm a teapot
  end
  
  get '/case_2' do
    @header['Example-Header'] = 'Example-Value'
    'Hello'
    # HTTP/1.1 200 OK
    # Server: Midori/1.0
    # Example-Header: Example-Value
    #
    # Hello
  end
  
  get '/case_3' do
    @status = 202
    @header['Example-Header'] = 'Example-Value'
    Midori::Response.new(status: 200, header: {}, body: 'Hello') # Overrides everything else
    # HTTP/1.1 200 OK
    # Server: Midori/1.0
    #
    # Hello
  end
end
```

