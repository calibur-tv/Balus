[产品需求文档](/)
#### calibur.tv 接口文档

> ##### 全局配置

1. 接口的`host`在 staging 环境是：[https://t-api.calibur.tv](https://t-api.calibur.tv)，production 下是：[https://api.calibur.tv](https://api.calibur.tv)
2. 用户认证使用 [JWT-Auth](https://auth0.com/docs/jwt)，需要用户认证的接口必须携带指定的 header 字段，例：
    ```javascript
    requestHeader = {
      'Authorization': 'Bearer ' + 'userTokenString'
    }
    ```
    `userTokenString`是用户登录成功后，服务端返回的一个随机字符串，客户端需要将其存起来，做用户验证
3. 移动端在发请求的时候，要额外携带两个参数做校验，例：
    ```javascript
    requestHeader = {
      'X-Auth-Timestamp': constAuthTimestamp,
      'X-Auth-Token': constAuthToken
    }
    ```
    `X-Auth-Timestamp` 和 `X-Auth-Token` 是在移动端自己生成，生成规则如下：
    ```javascript
    constAuthToken = md5('md5_salt' + constAuthTimestamp)
    ```
    `constAuthTimestamp` 是请求发出时的时间戳
    `md5_salt` 是加密的 key，这个 key 暂时先写死在 App 内，与服务端保持一致
4. 接口的返回统一如下：
    ```javascript
    response = {
     code: responseCode('type:integer'),
     message: responseMessage('type:array'),
     data: responseData('type:any')
    }
    ```
    无论请求成功或失败都会返回上述结构的数据，特别说明：
    > 1. 请求成功时，code 是 0，其它失败时，是 [http status code](https://baike.baidu.com/item/HTTP%E7%8A%B6%E6%80%81%E7%A0%81/5053660?fr=aladdin&fromid=11296236&fromtitle=HTTP+Status+Code) <br/>
    > 2. response 的 status code 在 code === 0 时是 200，其它时候与 response.code 一致 <br/>
    > 3. message 在返回成功时，是空数组`[]`，在返回失败时是失败的信息，将数组中的信息遍历之后按顺序展示即可，大部分情况下是一条信息 <br/>
    > 4. data 在返回成功时，会是任意可能的类型，返回失败时，是空字符串`''`
5. 接口超时设置为`30s`，如果超时则显示预定义的错误
6. `userTokenString`会过期，因此每次程序启动时，都需要发一个请求去刷新这个 token，刷新 token 的接口是：[refresh-user-api]()
7. 当 token 过了可刷新的时限时，即使请求上面的`refresh-user-api`也会报错，这时就需要重新登录了

#### 接口列表
##### [用户认证接口](/api/sign)