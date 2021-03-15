# node中使用redis存取session

### 新建项目安装相关依赖包

```shell
mkdir node_redis_session
cd node_redis_session
npm init -y
npm i express express-session uuid redis --save

```
#### 根据[express-session](https://github.com/expressjs/session#readme)官方文档的要求：
> Every session store must be an EventEmitter and implement specific methods. The following methods are the list of required, recommended, and optional.
> - Required methods are ones that this module will always call on the store.

1. *session store* 必须继承 *EventEmitter*
2. 必须有以下方法：
    - store.destroy(sid, callback)
    - store.get(sid, callback)
    - store.set(sid, session, callback)

### 直接上代码

#### app.js

```js
const express = require('express')
const {
    v4: uuidv4
} = require('uuid')
const session = require('express-session')
const RedisStore = require('./redis')(session)
const config = require('./config')

let app = express()

app.use(session({
    name: "session",
    secret: "session_@@key",
    genid: uuidv4,
    store: new RedisStore({
        client: config.RedisServer,
        prefix: 'userSession:'
    }),
    resave: false,
    rolling: true,
    saveUninitialized: true,
    cookie: {
        maxAge: 15 * 60 * 1000 //Session有效期
    }
}));

app.get('/', (req, res) => {
    console.log(req.sessionID)
    if (req.session.views) {
        req.session.views++
        res.setHeader('Content-Type', 'text/html')
        res.send(`<p>views: ${req.session.views}</p><p>expires in: ${req.session.cookie.maxAge / 1000}s</p>`)
    } else {
        req.session.views = 1
        res.send('welcome to the session demo. refresh!')
    }
})


app.listen(9090, () => {
    console.log('server running at 9090.')
})
```

#### redis.js
```js
const redis = require('redis')
const noop = () => undefined;

module.exports = function (session) {

    class RedisStore extends session.Store {
        constructor(options) {
            super(options)
            this.client = redis.createClient(options.client)
            this.prefix = options.prefix

            this.client.on('error', function (err) {
                console.log("command:" + err.command);
                console.log("args:", err.args);
                console.log("code:" + err.code);
            });

            this.client.on('connect', function () {
                console.log('sessionStore connected')
            });
        }

        get(sid, fn) {
            let store = this
            let psid = store.prefix + sid;
            if (!fn) fn = noop

            store.client.get(psid, function (er, data) {
                if (er) return fn(er)
                if (!data) return fn()

                try {
                    data = JSON.parse(data);
                } catch (er) {
                    return fn(er);
                }
                return fn(null, data);
            });
        }

        set(sid, sess, fn) {
            let store = this
            let sessionStr = store.prefix + sid
            let ttl = sess.cookie.maxAge
            let jsess = null

            if (!fn) fn = noop;

            try {
                jsess = JSON.stringify(sess);
            } catch (er) {
                return fn(er);
            }

            store.client.set([sessionStr, jsess, 'EX', ttl], function (er) {
                if (er) return fn(er);
                fn.apply(null, arguments);
            });
        }

        destroy(sid, fn) {
            sid = this.prefix + sid;
            this.client.del(sid, fn);
        }
    }
    return RedisStore
}
```

### config.js
```js
module.exports = {
    "RedisServer": {
        host: '192.168.1.99',
        port: 6379,
        pass: '',
        db: 1,
    }
}
```

### 实现效果
#### 第一次请求：
控制台和浏览器的结果：
<img src='https://ftp.bmp.ovh/imgs/2021/03/ec1fbe47b0ae0fe7.jpeg'/>

redis服务器的结果：
<img src='https://ftp.bmp.ovh/imgs/2021/03/c41f25116a7e94e5.jpeg'/>

#### 第二次请求：
控制台和浏览器的结果：
<img src='https://ftp.bmp.ovh/imgs/2021/03/41920c549ccb1135.jpeg'/>

redis服务器的结果：
<img src='https://ftp.bmp.ovh/imgs/2021/03/68519bd2892c9e2e.jpeg'/>

