# koa中使用passport 
  
passport可以实现用户注册检测，用户登录验证。支持本地账号验证和第三方账号登录验证

## 安装依赖

passport-local :必须安装  
koa-passport   ：根据项目框架安装

## 使用

使用策略验证用户登录
``` 
passport.use(new LocalStrategy(async function (username, password, done) {
  let where = {
    username
  }
  let result = await  UserModel.findOne(where);
  if (result != null) {
    if (result.password === password) {
      return done(null, result)
    } else {
      return done(null, false, '密码错误')
    }
  } else {
    return done(null, false, '用户不存在')
  }
}))
```  

让用户每次进入都通过session验证   

```
//序列化:查询到用户信息存储到session中
passport.serializeUser(function (user, done) {
  done(null, user)
})

//反序列化：每次请求时会在session中读取对象
passport.deserializeUser(function (user, done) {
  return done(null, user)
})

```