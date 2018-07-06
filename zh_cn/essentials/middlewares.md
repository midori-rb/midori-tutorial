# 中间件

中间件是 midori 所支持的一项令人激动的功能。许多 web 框架都已实现了这项功能。midori 的中间件与其他框架的表现有所不同，极大地降低了项目复杂度并提高了可扩展性。

## 基本用法

开始先继承一个 `Midori::Middleware` 类，示例如下:

```ruby
class JSONMiddleware < Midori::Middleware
  def before(request)
    request.body = JSON.parse(request.body) unless request.body == ''
    request
  end

  def after(_request, response)
    response.header['Content-Type'] = 'application/json'
    response.body = response.body.to_json
    response
  end
end
```

有两种方法在路由中使用中间件。
一种是通过 `use` 方法，这会影响当前作用域的所有路由。
另一种是通过 `filter` 方法，这只会对后面声明的路由生效。

示例如下：

```ruby
class API < Midori::API
  use AMiddleware

  filter BMiddleware
  get '/' do
    # Both AMiddleware and BMiddleware works here.
    'Hello'
  end

  get '/a' do
    # Only AMiddleware is involved here.
    'World'
  end
end
```

## 非栈式设计

对于 rack 用户，中间件被认为是栈的一部分。
在 Ruby 中栈过深会导致许多性能问题，包括增加上下文切换、线程切换、进程分叉以及其他一些情况时的消耗。
对于 Rails 这类的框架，其默认包含的许多中间件都会使这个问题变得更加严重。

在 midori 中， 中间件不再基于栈。
它使用了基于循环的系统来运行中间件代码。
这减少了中间件使用造成的弊端。

## 提前退出功能

midori 非栈式地支持了提前退出功能。
如果要触发它，仅需在请求处理期间响应一个 `Midori::Response` 对象。
这里提供了一个在 CORS 中间件中使用提前退出功能的示例。

```ruby
class CorsMiddleware < Midori::Middleware
  def before(request)
    if request.method == :OPTIONS
      # PREFLIGHT
      return Midori::Response.new(
        header: {
          'Access-Control-Allow-Origin': request.header['Origin'],
          'Access-Control-Request-Headers': 'Token',
          'Access-Control-Allow-Headers': 'Token',
          'Access-Control-Allow-Methods': 'OPTIONS, POST, PUT, DELETE'
        }
      )
    else
      request
    end
  end

  def after(request, response)
    response.header['Access-Control-Allow-Origin'] = request.header['Origin']
    response
  end
end
```
