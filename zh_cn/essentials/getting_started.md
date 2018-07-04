# 入门

## Hello Midori

Midori 是为用户能够轻松地进行基于 Ruby 的 web 与 API 开发而生的 [DSL](https://zh.wikipedia.org/wiki/%E9%A2%86%E5%9F%9F%E7%89%B9%E5%AE%9A%E8%AF%AD%E8%A8%80) ：

```ruby
# hello_midori.rb
require 'midori'

class HelloWorldAPI < Midori::API
  get '/' do
    'Ohayou Midori'
  end
end

Midori::Runner.new(HelloWorldAPI).start
```

输入以下命令来运行

```
$ ruby hello_midori.rb
```

在你的浏览器上访问 http://127.0.0.1:8080 , 如果看到显示 **Ohayou Midori** 的页面，表示你已成功用 Midori 完成了第一个应用。

