### [接口文档](/api/index)

#### 获取 geetest 验证码
> [Geetest文档](http://docs.geetest.com/)

```javascript
url = '/door/captcha'
method = 'POST'
authHeader = false
params = {}
response = {
  code: 0,
  message: [],
  data: {
    id: geetestGt('string'),  // 对应 geetest init 时的 gt
    access: geetestToken('string'), // 当请求需要做 geetest 认证时需要携带
    secret: geetestChallenge('string') // 对应 geetest 的 challenge
  }
}
```
`备注：`当请求需要做 geetest 认证时，你应该将接口返回的 data 对象存起来，然后将 data 对象作为接口的一个参数

#### 账号唯一性检验
```javascript
url = '/door/check/${method}/${access}'
method = 'POST'
authHeader = false
params = {}
response = {
  code: 0,
  message: [],
  data: count('integer') 
}
```
`method`是注册的方式，`phone | email`二选一，`access` 就是注册的账号，是邮箱或手机 <br/>
`备注：`如果账号没有被占用，data 是 0，如果被占用，data 是 1

#### 发送手机短信或邮箱验证码
```javascript
url = '/door/send'
method = 'POST'
authHeader = false
params = {
  method: registerMethod('required', 'phone | email'),
  access: registerAccess('required', '注册的账号，手机或邮箱'),
  nickname: registerUsername('required', '用户的昵称')
}
response = {
  code: 0,
  message: [],
  data: ''
}
```

#### 用户注册
```javascript
url = '/door/register'
method = 'POST'
authHeader = false
params = {
  method: registerMethod('required', 'phone | email'),
  access: registerAccess('required', '注册的账号，手机或邮箱'),
  nickname: registerUsername('required', '用户的昵称'),
  authCode: registerAuthCode('required', '用户收到的邮箱验证码或短信'),
  inviteCode: registerInviteCode('邀请码，非必选'),
  geetest: geetestObject('required', 'geetest对象，从服务端获取然后传回服务端')
}
response = {
  code: 0,
  message: [],
  data: jwtToken('string')
}
errorResponseCode = {
  401: '验证码过期',
  403: '账号已被占用'
}
```
`备注：`返回的 jwtToken 需要存起来，做用户认证

#### 用户登录
```javascript
url = '/door/login'
method = 'POST'
authHeader = false
params = {
  method: loginMethod('required', 'phone | email'),
  access: loginAccess('required', '手机或邮箱'),
  secret: loginAccess('required', '密码'),
  geetest: geetestObject('required', 'geetest对象，从服务端获取然后传回服务端'),
  remember: 1
}
response = {
  code: 0,
  message: [],
  data: jwtToken('string')
}
errorResponseCode = {
  422: '用户名或密码错误'
}
```

#### 获取用户信息
```javascript
url = '/door/user'
method = 'POST'
authHeader = true
params = {}
response = {
  code: 0,
  message: [],
  data: UserObject
}
errorResponse = {
  code: 0,
  message: [],
  data: null
}
```

#### 退出登录
```javascript
url = '/door/logout'
metohd = 'POST'
authHeader = true
params = {}
response = {
  code: 0,
  message: [],
  data: ''
}
```