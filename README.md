# nginx-lua-frequency

A frequency module for Nginx written in Lua

Installation
------------

```shell
luarocks install nginx-lua-frequency

```


Basic Usage
------------

```lua
local freqIns, err = require("frequency").init({
    freq = {
        -- seconds
        s = {
            unit = 30,
            rules = {
                r_30 = 100,
                r_60 = 120,
                r_90 = 150
            },
            max_window = 3
        },
        -- minutes
        m = {
            unit = 10,
            rules = {
                r_10 = 400,
                r_30 = 600
            },
            max_window = 3
        },
        -- hours
        h = {
            unit = 6,
            rules = {
                r_6 = 1000,
                r_12 = 1500
            },
            max_window = 2
        },
        rules_prefix = "r_",
        expire = 43210 -- 12 hours + 10 seconds
    },
    adapter = require("frequency.adapter.memcached"):connect("127.0.0.1", 11211, 5, "MEMCACHED_")
    after_hit = "log" -- forbid|return|header|log -- 命中规则后的下一步处理
})

if err ~= nil then
    ngx.log(ngx.ERR, "frequency init error: ", err)
    return
end

freqIns:check(ngx.var.remote_addr)
```


License
--------

licensed under the MIT License - see the `LICENSE` file for details
