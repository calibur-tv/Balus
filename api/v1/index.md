FORMAT: 1A

# calibur.tv API docs

# 用户认证相关接口

## 发送手机验证码 [POST /door/message]
一个通用的接口，通过 `type` 和 `phone_number` 发送手机验证码.
目前支持 `type` 为：
1. `sign_up`，注册时调用
2. `forgot_password`，找回密码时使用

+ Request (application/json)
    + Body

            {
                "type": "上面的某个type",
                "phone_number": "只支持11位的手机号",
                "geetest": "Geetest认证对象"
            }

+ Response 201 (application/json)
    + Body

            {
                "code": 0,
                "data": "短信已发送"
            }

+ Response 400 (application/json)
    + Body

            {
                "code": 40001,
                "message": "未经过图形验证码认证"
            }

+ Response 401 (application/json)
    + Body

            {
                "code": 40100,
                "message": "图形验证码认证失败"
            }

+ Response 400 (application/json)
    + Body

            {
                "code": 40003,
                "message": "各种错误"
            }

+ Response 503 (application/json)
    + Body

            {
                "code": 50310,
                "message": "短信服务暂不可用或请求过于频繁"
            }

## 用户注册 [POST /door/register]
目前仅支持使用手机号注册

+ Request (application/json)
    + Body

            {
                "access": "手机号",
                "secret": "6至16位的密码",
                "nickname": "昵称，只能包含汉字、数字和字母",
                "authCode": "6位数字的短信验证码"
            }

+ Response 201 (application/json)
    + Body

            {
                "code": 0,
                "data": "JWT-Token"
            }

+ Response 400 (application/json)
    + Body

            {
                "code": 40003,
                "message": "各种错误"
            }

## 用户登录 [POST /door/login]
目前仅支持手机号和密码登录

+ Request (application/json)
    + Body

            {
                "access": "手机号",
                "secret": "密码",
                "geetest": "Geetest认证对象"
            }

+ Response 200 (application/json)
    + Body

            {
                "code": 0,
                "data": "JWT-Token"
            }

+ Response 400 (application/json)
    + Body

            {
                "code": 40001,
                "message": "未经过图形验证码认证"
            }

+ Response 401 (application/json)
    + Body

            {
                "code": 40100,
                "message": "图形验证码认证失败"
            }

+ Response 400 (application/json)
    + Body

            {
                "code": 40003,
                "message": "各种错误"
            }

## 用户登出 [POST /door/logout]


+ Request (application/json)
    + Headers

            Authorization: Bearer JWT-Token

+ Response 204 (application/json)

## 获取用户信息 [POST /door/user]
每次`启动应用`、`登录`、`注册`成功后调用

+ Request (application/json)
    + Headers

            Authorization: Bearer JWT-Token

+ Response 200 (application/json)
    + Body

            {
                "code": 0,
                "data": "用户对象"
            }

+ Response 401 (application/json)
    + Body

            {
                "code": 40104,
                "message": "未登录的用户"
            }

# 番剧相关接口

## 获取番剧时间轴 [GET /bangumi/timeline]


+ Parameters
    + year: (string, required) - 从哪一年开始获取
    + take: (string, optional) - 一次获取几年的内容
        + Default: 3

+ Response 200 (application/json)
    + Body

            {
                "code": 0,
                "data": {
                    "list": "番剧列表",
                    "min": "可获取到数据的最小年份"
                }
            }

+ Response 400 (application/json)
    + Body

            {
                "code": 40003,
                "message": "没有传年份",
                "data": ""
            }

## 获取新番列表 [GET /bangumi/released]


+ Response 200 (application/json)
    + Body

            {
                "code": 0,
                "data": "番剧列表"
            }

## 获取所有的番剧标签 [GET /bangumi/tags]


+ Response 200 (application/json)
    + Body

            {
                "code": 0,
                "data": "标签列表"
            }

## 根据参数获取番剧列表 [GET /bangumi/category]


+ Parameters
    + id: (string, required) - 选中的标签id，用 - 链接的字符串
    + page: (string, required) - 页码
        + Default: 1

+ Response 200 (application/json)
    + Body

            {
                "code": 0,
                "data": "番剧列表"
            }

+ Response 400 (application/json)
    + Body

            {
                "code": 40003,
                "message": "请求参数错误",
                "data": ""
            }

## 获取番剧详情 [GET /bangumi/${bangumiId}/show]


+ Request (application/json)
    + Headers

            Authorization: Bearer JWT-Token

+ Response 200 (application/json)
    + Body

            {
                "code": 0,
                "data": "番剧对象"
            }

+ Response 404 (application/json)
    + Body

            {
                "code": 40401,
                "message": "不存在的番剧",
                "data": ""
            }

## 获取番剧视频 [GET /bangumi/${bangumiId}/videos]


+ Response 200 (application/json)
    + Body

            {
                "code": 0,
                "data": {
                    "videos": "视频列表",
                    "repeat": "视频集数是否连续",
                    "total": "视频总数"
                }
            }

+ Response 404 (application/json)
    + Body

            {
                "code": 40401,
                "message": "不存在的番剧",
                "data": ""
            }

## 关注或取消关注番剧 [POST /bangumi/${bangumiId}/follow]


+ Request (application/json)
    + Headers

            Authorization: Bearer JWT-Token

+ Response 201 (application/json)
    + Body

            {
                "code": 0,
                "data": "是否已关注"
            }

+ Response 401 (application/json)
    + Body

            {
                "code": 40104,
                "message": "用户认证失败",
                "data": ""
            }

## 获取关注番剧的用户列表 [POST /bangumi/${bangumiId}/followers]


+ Request (application/json)
    + Body

            {
                "seenIds": "看过的userIds，用','隔开的字符串",
                "take": "获取数量"
            }

+ Response 200 (application/json)
    + Body

            {
                "code": 0,
                "data": "用户列表"
            }

## 番剧的帖子列表 [POST /bangumi/${bangumiId}/posts]


+ Request (application/json)
    + Headers

            Authorization: Bearer JWT-Token
    + Body

            {
                "seenIds": "看过的userIds，用','隔开的字符串",
                "take": "获取数量"
            }

+ Response 200 (application/json)
    + Body

            {
                "code": 0,
                "data": {
                    "list": "番剧列表",
                    "total": "总数"
                }
            }

# 动漫角色相关接口

## 获取番剧角色列表 [GET /bangumi/${bangumiId}/roles]


+ Response 200 (application/json)
    + Body

            {
                "code": 0,
                "data": "角色列表"
            }

+ Response 404 (application/json)
    + Body

            {
                "code": 40003,
                "message": "不存在的番剧",
                "data": ""
            }

# 视频相关接口

## 获取视频资源 [GET /video/${videoId}/show]


+ Response 200 (application/json)
    + Body

            {
                "code": 0,
                "data": {
                    "info": "视频对象",
                    "bangumi": "番剧信息",
                    "list": {
                        "total": "视频总数",
                        "repeat": "是否重排",
                        "videos": "视频列表"
                    }
                }
            }

+ Response 404 (application/json)
    + Body

            {
                "code": 40401,
                "message": "不存在的视频资源",
                "data": ""
            }

## 记录视频播放信息 [GET /video/${videoId}/playing]


+ Request (application/json)
    + Headers

            Authorization: Bearer JWT-Token

# 用户相关接口

## 用户每日签到 [POST /user/daySign]


+ Request (application/json)
    + Headers

            Authorization: Bearer JWT-Token

+ Response 204 (application/json)

+ Response 401 (application/json)
    + Body

            {
                "code": 40104,
                "message": "未登录的用户",
                "data": ""
            }

+ Response 403 (application/json)
    + Body

            {
                "code": 40301,
                "message": "今日已签到",
                "data": ""
            }

## 更新用户资料中的图片 [POST /user/setting/image]


+ Request (application/json)
    + Headers

            Authorization: Bearer JWT-Token
    + Body

            {
                "type": "avatar或banner",
                "url": "图片地址"
            }

+ Response 204 (application/json)

+ Response 400 (application/json)
    + Body

            {
                "code": 40003,
                "message": "请求参数错误",
                "data": ""
            }

+ Response 401 (application/json)
    + Body

            {
                "code": 40104,
                "message": "未登录的用户",
                "data": ""
            }

## 获取用户页面信息 [POST /user/${userZone}/show]


+ Request (application/json)
    + Headers

            Authorization: Bearer JWT-Token

+ Response 200 (application/json)
    + Body

            {
                "code": 0,
                "data": "用户信息对象"
            }

+ Response 404 (application/json)
    + Body

            {
                "code": 40401,
                "message": "该用户不存在",
                "data": ""
            }

## 修改用户自己的信息 [POST /user/setting/profile]


+ Request (application/json)
    + Headers

            Authorization: Bearer JWT-Token
    + Body

            {
                "sex": "性别: 0, 1, 2, 3, 4",
                "signature": "用户签名，最多20字",
                "nickname": "用户昵称，最多14个字符",
                "birthday": "以秒为单位的时间戳"
            }

+ Response 204 (application/json)

+ Response 404 (application/json)
    + Body

            {
                "code": 40104,
                "message": "该用户不存在",
                "data": ""
            }

## 用去用户关注番剧的列表 [POST /user/${userZone}/followed/bangumi]


+ Response 200 (application/json)
    + Body

            {
                "code": 0,
                "data": "番剧列表"
            }

+ Response 404 (application/json)
    + Body

            {
                "code": 40104,
                "message": "该用户不存在",
                "data": ""
            }

## 用户发布的帖子列表 [POST /user/${userZone}/posts/mine]


+ Request (application/json)
    + Body

            {
                "seenIds": "看过的postIds, 用','分割的字符串",
                "take": "获取的数量"
            }

+ Response 200 (application/json)
    + Body

            {
                "code": 0,
                "data": "帖子列表"
            }

+ Response 404 (application/json)
    + Body

            {
                "code": 40104,
                "message": "找不到用户",
                "data": ""
            }

## 用户回复的帖子列表 [POST /user/${userZone}/posts/reply]


+ Request (application/json)
    + Body

            {
                "seenIds": "看过的postIds, 用','分割的字符串",
                "take": "获取的数量"
            }

+ Response 200 (application/json)
    + Body

            {
                "code": 0,
                "data": "帖子列表"
            }

+ Response 404 (application/json)
    + Body

            {
                "code": 40104,
                "message": "找不到用户",
                "data": ""
            }

## 用户喜欢的帖子列表 [POST /user/${userZone}/posts/mine]


+ Request (application/json)
    + Body

            {
                "seenIds": "看过的postIds, 用','分割的字符串",
                "take": "获取的数量"
            }

+ Response 200 (application/json)
    + Body

            {
                "code": 0,
                "data": "帖子列表"
            }

+ Response 404 (application/json)
    + Body

            {
                "code": 40104,
                "message": "找不到用户",
                "data": ""
            }

## 用户收藏的帖子列表 [POST /user/${userZone}/posts/mine]


+ Request (application/json)
    + Body

            {
                "seenIds": "看过的postIds, 用','分割的字符串",
                "take": "获取的数量"
            }

+ Response 200 (application/json)
    + Body

            {
                "code": 0,
                "data": "帖子列表"
            }

+ Response 404 (application/json)
    + Body

            {
                "code": 40104,
                "message": "找不到用户",
                "data": ""
            }

## 用户反馈 [POST /user/feedback]


+ Request (application/json)
    + Body

            {
                "type": "反馈的类型",
                "desc": "反馈内容，最多120字"
            }

+ Response 204 (application/json)

+ Response 400 (application/json)
    + Body

            {
                "code": 40003,
                "message": "请求参数错误",
                "data": ""
            }

## 用户消息列表 [POST /user/notifications/list]


+ Request (application/json)
    + Body

            {
                "take": "获取个数",
                "minId": "看过的最小id"
            }

+ Response 200 (application/json)
    + Body

            {
                "code": 0,
                "data": "消息列表"
            }

+ Response 401 (application/json)
    + Body

            {
                "code": 40104,
                "message": "未登录的用户",
                "data": ""
            }

## 用户未读消息个数 [POST /user/notifications/count]


+ Response 200 (application/json)
    + Body

            {
                "code": 0,
                "data": "未读个数"
            }

+ Response 401 (application/json)
    + Body

            {
                "code": 40104,
                "message": "未登录的用户",
                "data": ""
            }

## 读取某条消息 [POST /user/notifications/read]


+ Request (application/json)
    + Body

            {
                "id": "消息id"
            }

+ Response 204 (application/json)

+ Response 401 (application/json)
    + Body

            {
                "code": 40104,
                "message": "未登录的用户",
                "data": ""
            }

+ Response 404 (application/json)
    + Body

            {
                "code": 40401,
                "message": "不存在的消息",
                "data": ""
            }

+ Response 403 (application/json)
    + Body

            {
                "code": 40301,
                "message": "没有权限进行操作",
                "data": ""
            }

# 帖子相关接口

## 新建帖子 [POST /post/create]


+ Request (application/json)
    + Headers

            Authorization: Bearer JWT-Token
    + Body

            {
                "title": "标题，不超过40个字",
                "bangumiId": "番剧id",
                "content": "帖子内容，不超过1000个字",
                "desc": "帖子描述，不超过120个字",
                "images": "帖子图片列表"
            }

+ Response 201 (application/json)
    + Body

            {
                "code": 0,
                "data": "帖子id"
            }

+ Response 400 (application/json)
    + Body

            {
                "code": 40003,
                "message": "请求参数错误",
                "data": "错误详情"
            }

+ Response 401 (application/json)
    + Body

            {
                "code": 40104,
                "message": "未登录的用户",
                "data": ""
            }

## 帖子信息 [POST /post/${postId}/show]


+ Request A (application/json)
    + Headers

            Authorization: Bearer JWT-Token
    + Body

            {
                "take": "获取数量",
                "only": "是否只看楼主"
            }

+ Response 200 (application/json)
    + Body

            {
                "code": 0,
                "0": {
                    "post": "主题帖信息",
                    "list": "帖子列表",
                    "bangumi": "帖子番剧信息",
                    "user": "楼主信息",
                    "total": "帖子总数"
                }
            }

+ Request B (application/json)
    + Headers

            Authorization: Bearer JWT-Token
    + Body

            {
                "seenIds": "看过的postIds，用','隔开的字符串",
                "take": "获取数量",
                "only": "是否只看楼主"
            }

+ Response 200 (application/json)
    + Body

            {
                "code": 0,
                "data": {
                    "list": "帖子列表",
                    "total": "帖子总数"
                }
            }

+ Response 400 (application/json)
    + Body

            {
                "code": 40004,
                "message": "不是主题帖",
                "data": ""
            }

+ Response 404 (application/json)
    + Body

            {
                "code": 40401,
                "message": "不存在的帖子",
                "data": ""
            }

## 回复主题帖 [POST /post/${postId}/reply]


+ Request (application/json)
    + Headers

            Authorization: Bearer JWT-Token
    + Body

            {
                "content": "帖子内容，不超过1000个字",
                "images": "帖子图片列表"
            }

+ Response 201 (application/json)
    + Body

            {
                "code": 0,
                "data": "帖子对象"
            }

+ Response 400 (application/json)
    + Body

            {
                "code": 40003,
                "message": "请求参数错误",
                "data": "错误详情"
            }

+ Response 401 (application/json)
    + Body

            {
                "code": 40104,
                "message": "未登录的用户",
                "data": ""
            }

+ Response 404 (application/json)
    + Body

            {
                "code": 40401,
                "message": "不存在的帖子",
                "data": ""
            }

## 获取给帖子点赞的用户列表 [POST /post/${postId}/likeUsers]


+ Request (application/json)
    + Body

            {
                "seenIds": "看过的userIds, 用','分割的字符串",
                "take": "获取的数量"
            }

+ Response 200 (application/json)
    + Body

            {
                "code": 0,
                "data": "用户列表"
            }

+ Response 404 (application/json)
    + Body

            {
                "code": 40401,
                "message": "不存在的帖子",
                "data": ""
            }

## 给帖子点赞或取消点赞 [POST /post/${postId}/toggleLike]


+ Request (application/json)
    + Headers

            Authorization: Bearer JWT-Token

+ Response 201 (application/json)
    + Body

            {
                "code": 0,
                "data": "是否已赞"
            }

+ Response 401 (application/json)
    + Body

            {
                "code": 40104,
                "message": "未登录的用户",
                "data": ""
            }

+ Response 403 (application/json)
    + Body

            {
                "code": 40301,
                "message": "不能给自己点赞/金币不足/请求错误",
                "data": ""
            }

+ Response 404 (application/json)
    + Body

            {
                "code": 40401,
                "message": "内容已删除",
                "data": ""
            }

## 收藏主题帖或取消收藏 [POST /post/${postId}/toggleMark]


+ Request (application/json)
    + Headers

            Authorization: Bearer JWT-Token

+ Response 201 (application/json)
    + Body

            {
                "code": 0,
                "data": "是否已收藏"
            }

+ Response 401 (application/json)
    + Body

            {
                "code": 40104,
                "message": "未登录的用户",
                "data": ""
            }

+ Response 403 (application/json)
    + Body

            {
                "code": 40301,
                "message": "不能收藏自己的帖子/不是主题帖",
                "data": ""
            }

+ Response 404 (application/json)
    + Body

            {
                "code": 40401,
                "message": "不存在的帖子",
                "data": ""
            }

## 删除帖子 [POST /post/${postId}/deletePost]


+ Request (application/json)
    + Headers

            Authorization: Bearer JWT-Token

+ Response 204 (application/json)

+ Response 401 (application/json)
    + Body

            {
                "code": 40104,
                "message": "未登录的用户",
                "data": ""
            }

+ Response 403 (application/json)
    + Body

            {
                "code": 40301,
                "message": "权限不足",
                "data": ""
            }

+ Response 404 (application/json)
    + Body

            {
                "code": 40401,
                "message": "不存在的帖子",
                "data": ""
            }

## 最新帖子列表 [POST /trending/post/new]


+ Request A (application/json)
    + Headers

            Authorization: Bearer JWT-Token
    + Body

            {
                "take": "获取数量",
                "seenIds": "看过的postIds, 用','号分割的字符串"
            }

+ Response 200 (application/json)
    + Body

            {
                "code": 0,
                "0": {
                    "data": "帖子列表"
                }
            }

## 热门帖子列表 [POST /trending/post/hot]


+ Request A (application/json)
    + Headers

            Authorization: Bearer JWT-Token
    + Body

            {
                "take": "获取数量",
                "seenIds": "看过的postIds, 用','号分割的字符串"
            }

+ Response 200 (application/json)
    + Body

            {
                "code": 0,
                "0": {
                    "data": "帖子列表"
                }
            }

# 图片相关接口

## 获取首页banner图 [GET /image/banner]


+ Response 200 (application/json)
    + Body

            {
                "code": 0,
                "data": "图片列表"
            }

## 获取 Geetest 验证码 [POST /image/captcha]


+ Response 200 (application/json)
    + Body

            {
                "code": 0,
                "data": {
                    "id": "Geetest.gt",
                    "secret": "Geetest.challenge",
                    "access": "认证密匙"
                }
            }

## 获取图片上传token [POST /image/uptoken]


+ Request (application/json)
    + Headers

            Authorization: Bearer JWT-Token

+ Response 200 (application/json)
    + Body

            {
                "code": 0,
                "data": {
                    "upToken": "上传图片的token",
                    "expiredAt": "token过期时间戳，单位为s"
                }
            }

+ Response 401 (application/json)
    + Body

            {
                "code": 40104,
                "message": "未登录的用户",
                "data": ""
            }