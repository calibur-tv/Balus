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
3. 移动端在发请求的时候，要额外携带以下参数做校验：
    ```javascript
    requestHeader = {
      'Accept': 'application/x.api.latest+json'
    }
    ```
    `Accept`中的`latest`要替换为当前 API 的版本号，如 v1，v2...
4. 接口的返回统一如下：
    ```javascript
    response = {
     code: responseCode('type:integer'),
     data: responseData('type:any')
    }
    ```
    无论请求成功或失败都会返回上述结构的数据，特别说明：
    > 1. 请求成功时，code 是 0，其它失败时，是 [http status code](https://baike.baidu.com/item/HTTP%E7%8A%B6%E6%80%81%E7%A0%81/5053660?fr=aladdin&fromid=11296236&fromtitle=HTTP+Status+Code) <br/>
    > 2. response 的 status code 在 code === 0 时是 200，其它时候与 response.code 一致 <br/>
    > 4. data 在返回成功时，会是任意可能的类型，返回失败时，是错误信息字符串
5. 接口超时设置为`10s`，如果超时则显示预定义的错误

#### 接口列表
##### [v1版接口](/api/v1/index)