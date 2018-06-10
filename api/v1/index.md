FORMAT: 1A

# calibur.tv API docs

# 搜索相关接口

## 根据关键字搜索番剧 [GET /search/index]


+ Parameters
    + q: (string, required) - 关键字

+ Response 200 (application/json)
    + Body

            {
                "code": 0,
                "data": "番剧的 url"
            }

# 用户认证相关接口

## 发送手机验证码 [POST /door/message]
> 一个通用的接口，通过 `type` 和 `phone_number` 发送手机验证码.
目前支持 `type` 为：
1. `sign_up`，注册时调用
2. `forgot_password`，找回密码时使用

> 目前返回的数字验证码是`6位`

+ Parameters
    + type: (string, required) - 上面的某种type
    + phone_number: (number, required) - 只支持`11位`的手机号
    + geetest: (object, required) - Geetest认证对象

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

+ Parameters
    + access: (number, required) - 手机号
    + secret: (string, required) - `6至16位`的密码
    + nickname: (string, required) - 昵称，只能包含`汉字、数字和字母，2~14个字符组成，1个汉字占2个字符`
    + authCode: (number, required) - 6位数字的短信验证码

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

+ Parameters
    + access: (number, required) - 手机号
    + secret: (string, required) - 6至16位的密码
    + geetest: (object, required) - Geetest认证对象

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

## 重置密码 [POST /door/reset]


+ Parameters
    + access: (number, required) - 手机号
    + secret: (string, required) - 6至16位的密码
    + authCode: (number, required) - 6位数字的短信验证码

+ Response 200 (application/json)
    + Body

            {
                "code": 0,
                "data": "密码重置成功"
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

# 番剧相关接口

## 番剧时间轴 [GET /bangumi/timeline]


+ Parameters
    + year: (integer, required) - 从哪一年开始获取

+ Response 200 (application/json)
    + Body

            {
                "code": 0,
                "data": {
                    "list": "番剧列表",
                    "noMore": "没有更多了"
                }
            }

+ Response 400 (application/json)
    + Body

            {
                "code": 40003,
                "message": "参数错误"
            }

## 新番列表（周） [GET /bangumi/released]


+ Response 200 (application/json)
    + Body

            {
                "code": 0,
                "data": "番剧列表"
            }

## 所有的番剧标签 [GET /bangumi/tags]


+ Response 200 (application/json)
    + Body

            {
                "code": 0,
                "data": "标签列表"
            }

## 根据标签获取番剧列表 [GET /bangumi/category]


+ Parameters
    + id: (string, required) - 选中的标签id，`用 - 链接的字符串`
    + page: (integer, required) - 页码
        + Default: 0

+ Response 200 (application/json)
    + Body

            {
                "code": 0,
                "data": {
                    "list": "番剧列表",
                    "total": "该标签下番剧的总数",
                    "noMore": "是否没有更多了"
                }
            }

+ Response 400 (application/json)
    + Body

            {
                "code": 40003,
                "message": "请求参数错误"
            }

## 番剧详情 [GET /bangumi/`bangumiId`/show]


+ Response 200 (application/json)
    + Body

            {
                "code": 0,
                "data": "番剧信息"
            }

+ Response 404 (application/json)
    + Body

            {
                "code": 40401,
                "message": "不存在的番剧"
            }

## 番剧视频 [GET /bangumi/`bangumiId`/videos]


+ Response 200 (application/json)
    + Body

            {
                "code": 0,
                "data": {
                    "videos": "视频列表",
                    "has_season": "是否有多个季度",
                    "total": "视频总数"
                }
            }

+ Response 404 (application/json)
    + Body

            {
                "code": 40401,
                "message": "不存在的番剧"
            }

## 关注或取消关注番剧 [POST /bangumi/`bangumiId`/toggleFollow]


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
                "message": "用户认证失败"
            }

## 关注番剧的用户列表 [GET /bangumi/`bangumiId`/followers]


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
                "data": {
                    "list": "用户列表",
                    "noMore": "没有更多"
                }
            }

## 番剧的帖子列表 [GET /bangumi/`bangumiId`/posts/news]


+ Parameters
    + maxId: (integer, required) - 看过的帖子里，最大的一个id

+ Response 200 (application/json)
    + Body

            {
                "code": 0,
                "data": {
                    "list": "番剧列表",
                    "total": "总数",
                    "noMore": "没有更多"
                }
            }

## 番剧的漫画列表 [GET /bangumi/`bangumiId`/cartoon]


+ Parameters
    + page: (integer, required) - 页码
        + Default: 0

+ Response 200 (application/json)
    + Body

            {
                "code": 0,
                "data": {
                    "list": "漫画列表",
                    "total": "总数",
                    "noMore": "没有更多"
                }
            }

# 动漫角色相关接口

## 番剧角色列表 [GET /bangumi/`bangumiId`/roles]


+ Response 200 (application/json)
    + Body

            {
                "code": 0,
                "data": "角色列表"
            }

+ Response 404 (application/json)
    + Body

            {
                "code": 40401,
                "message": "不存在的番剧"
            }

## 给角色应援 [POST /bangumi/`bangumiId`/role/`roleId`/star]


+ Request (application/json)
    + Headers

            Authorization: Bearer JWT-Token

+ Response 204 (application/json)

+ Response 404 (application/json)
    + Body

            {
                "code": 40401,
                "message": "不存在的角色"
            }

+ Response 403 (application/json)
    + Body

            {
                "code": 40301,
                "message": "没有足够的金币"
            }

## 角色详情 [GET /cartoon_role/`roleId`/show]


+ Response 200 (application/json)
    + Body

            {
                "code": 0,
                "data": {
                    "bangumi": "番剧简介",
                    "data": "角色信息"
                }
            }

+ Response 404 (application/json)
    + Body

            {
                "code": 40401,
                "message": "不存在的角色"
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
                "message": "不存在的视频资源"
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
                "message": "未登录的用户"
            }

+ Response 403 (application/json)
    + Body

            {
                "code": 40301,
                "message": "今日已签到"
            }

## 更新用户资料中的图片 [POST /user/setting/image]


+ Parameters
    + type: (string, required) - `avatar`或`banner`
    + url: (string, required) - 图片地址，不带`host`

+ Request (application/json)
    + Headers

            Authorization: Bearer JWT-Token

+ Response 204 (application/json)

## 用户详情 [GET /user/`zone`/show]


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
                "message": "该用户不存在"
            }

## 用户关注的番剧列表 [GET /user/`zone`/followed/bangumi]


+ Response 200 (application/json)
    + Body

            {
                "code": 0,
                "data": "番剧列表"
            }

+ Response 404 (application/json)
    + Body

            {
                "code": 40401,
                "message": "该用户不存在"
            }

## 用户发布的帖子列表 [GET /user/`zone`/posts/mine]


+ Request (application/json)
    + Body

            {
                "minId": "看过的帖子列表里，id 最小的那个帖子的 id"
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
                "code": 40401,
                "message": "找不到用户"
            }

## 用户回复的帖子列表 [GET /user/`zone`/posts/reply]


+ Request (application/json)
    + Body

            {
                "minId": "看过的帖子列表里，id 最小的那个帖子的 id"
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
                "code": 40401,
                "message": "找不到用户"
            }

## 用户喜欢的帖子列表 [GET /user/`zone`/posts/like]


+ Response 200 (application/json)
    + Body

            {
                "code": 0,
                "data": "帖子列表"
            }

+ Response 404 (application/json)
    + Body

            {
                "code": 40401,
                "message": "找不到用户"
            }

## 用户收藏的帖子列表 [GET /user/`zone`/posts/mark]


+ Response 200 (application/json)
    + Body

            {
                "code": 0,
                "data": "帖子列表"
            }

+ Response 404 (application/json)
    + Body

            {
                "code": 40401,
                "message": "找不到用户"
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
                "message": "请求参数错误"
            }

## 用户消息列表 [GET /user/notifications/list]


+ Request (application/json)
    + Body

            {
                "minId": "看过的最小id"
            }

+ Request (application/json)
    + Headers

            Authorization: Bearer JWT-Token

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
                "message": "未登录的用户"
            }

## 用户未读消息个数 [GET /user/notifications/count]


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
                "message": "未登录的用户"
            }

## 读取某条消息 [POST /user/notifications/read]


+ Request (application/json)
    + Body

            {
                "id": "消息id"
            }

+ Request (application/json)
    + Headers

            Authorization: Bearer JWT-Token

+ Response 204 (application/json)

## 清空未读消息 [POST /user/notifications/clear]


+ Request (application/json)
    + Headers

            Authorization: Bearer JWT-Token

+ Response 204 (application/json)

## 用户应援的角色列表 [GET /user/`zone`/followed/role]


+ Parameters
    + page: (integer, required) - 页码
        + Default: 0

+ Response 200 (application/json)
    + Body

            {
                "code": 0,
                "data": {
                    "list": "角色列表",
                    "total": "总数",
                    "noMore": "没有更多"
                }
            }

+ Response 404 (application/json)
    + Body

            {
                "code": 40401,
                "message": "该用户不存在"
            }

## 用户的图片相册列表 [GET /user/images/albums]


+ Request (application/json)
    + Headers

            Authorization: Bearer JWT-Token

+ Response 200 (application/json)
    + Body

            {
                "code": 0,
                "data": "相册列表"
            }

# 帖子相关接口

## 新建帖子 [POST /post/create]
> 图片对象示例：
1. `key` 七牛传图后得到的 key，不包含图片地址的 host，如一张图片 image.calibur.tv/user/1/avatar.png，七牛返回的 key 是：user/1/avatar.png，将这个 key 传到后端
2. `width` 图片的宽度，七牛上传图片后得到
3. `height` 图片的高度，七牛上传图片后得到
4. `size` 图片的尺寸，七牛上传图片后得到
5. `type` 图片的类型，七牛上传图片后得到

+ Parameters
    + bangumiId: (integer, required) - 所选的番剧 id
    + title: (string, required) - 标题`40字以内`
    + desc: (string, required) - content可能是富文本，desc是`120字以内的纯文本`
    + content: (string, required) - 内容，`1000字以内`
    + images: (array, required) - 图片对象数组

+ Request (application/json)
    + Headers

            Authorization: Bearer JWT-Token

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

## 回复主题帖 [POST /post/`postId`/reply]


+ Parameters
    + content: (string, required) - 内容，`1000字以内`
    + images: (array, required) - 图片对象数组

+ Request (application/json)
    + Headers

            Authorization: Bearer JWT-Token

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
                "message": "请求参数错误"
            }

+ Response 401 (application/json)
    + Body

            {
                "code": 40104,
                "message": "未登录的用户"
            }

+ Response 404 (application/json)
    + Body

            {
                "code": 40401,
                "message": "不存在的帖子"
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
                "message": "不存在的帖子"
            }

## 给帖子点赞或取消点赞 [POST /post/`postId`/toggleLike]


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
                "message": "未登录的用户"
            }

+ Response 403 (application/json)
    + Body

            {
                "code": 40301,
                "message": "不能给自己点赞/金币不足/请求错误"
            }

+ Response 404 (application/json)
    + Body

            {
                "code": 40401,
                "message": "内容已删除"
            }

## 收藏主题帖或取消收藏 [POST /post/`postId`/toggleMark]


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
                "message": "未登录的用户"
            }

+ Response 403 (application/json)
    + Body

            {
                "code": 40301,
                "message": "不能收藏自己的帖子"
            }

+ Response 404 (application/json)
    + Body

            {
                "code": 40401,
                "message": "不存在的帖子"
            }

## 删除帖子 [POST /post/`postId`/deletePost]


+ Request (application/json)
    + Headers

            Authorization: Bearer JWT-Token

+ Response 204 (application/json)

+ Response 401 (application/json)
    + Body

            {
                "code": 40104,
                "message": "未登录的用户"
            }

+ Response 403 (application/json)
    + Body

            {
                "code": 40301,
                "message": "权限不足"
            }

+ Response 404 (application/json)
    + Body

            {
                "code": 40401,
                "message": "不存在的帖子"
            }

## 删除帖子的某一层 [POST /post/`postId`/deleteComment]


+ Parameters
    + commentId: (integer, required) - 楼层帖子的 id

+ Request (application/json)
    + Headers

            Authorization: Bearer JWT-Token

+ Response 204 (application/json)

+ Response 401 (application/json)
    + Body

            {
                "code": 40104,
                "message": "未登录的用户"
            }

+ Response 403 (application/json)
    + Body

            {
                "code": 40301,
                "message": "继续操作前请先登录"
            }

+ Response 404 (application/json)
    + Body

            {
                "code": 40401,
                "message": "不存在的帖子|该评论已被删除"
            }

## 最新帖子列表 [GET /post/trending/news]


+ Parameters
    + minId: (integer, required) - 看过的帖子里，id 最小的一个

+ Response 200 (application/json)
    + Body

            {
                "code": 0,
                "data": {
                    "list": "帖子列表",
                    "total": "总数",
                    "noMore": "没有更多了"
                }
            }

## 动态帖子列表 [GET /post/trending/active]


+ Parameters
    + minId: (integer, required) - 看过的帖子里，id 最小的一个

+ Response 200 (application/json)
    + Body

            {
                "code": 0,
                "data": {
                    "list": "帖子列表",
                    "total": "总数",
                    "noMore": "没有更多了"
                }
            }

## 热门帖子列表 [GET /post/trending/hot]


+ Parameters
    + seenIds: (string, required) - 看过的帖子的`ids`, 用','号分割的字符串

+ Response 200 (application/json)
    + Body

            {
                "code": 0,
                "data": {
                    "list": "帖子列表",
                    "total": "总数",
                    "noMore": "没有更多了"
                }
            }

# 评论相关接口

## 子评论列表 [GET /`type`/comment/`id`/list]
> 一个通用的接口，通过 `type` 和 `commentId` 来获取子评论列表.
目前支持 `type` 为：
1. `post`，帖子

> `commentId`是父评论的 id：
1. `父评论` 一部视频下的评论列表，列表中的每一个就是一个父评论
2. `子评论` 每个父评论都有回复列表，这个回复列表中的每一个就是子评论

+ Parameters
    + type: (string, required) - 上面的某种 type
    + commentId: (integer, required) - 父评论 id
    + page: (integer, required) - 页码
        + Default: 0

+ Response 200 (application/json)
    + Body

            {
                "code": 0,
                "data": "评论列表"
            }

+ Response 400 (application/json)
    + Body

            {
                "code": 40003,
                "message": "没有页码|错误的类型"
            }

+ Response 404 (application/json)
    + Body

            {
                "code": 40401,
                "message": "不存在的评论"
            }

## 回复评论 [POST /`type`/comment/`commentId`/reply]


+ Parameters
    + type: (string, required) - 上面的某种 type
    + commentId: (integer, required) - 父评论 id
    + targetUserId: (integer, required) - 父评论的用户 id
    + content: (string, required) - 评论内容，`纯文本，100字以内`

+ Request (application/json)
    + Headers

            Authorization: Bearer JWT-Token

+ Response 201 (application/json)
    + Body

            {
                "code": 0,
                "data": "子评论对象"
            }

+ Response 400 (application/json)
    + Body

            {
                "code": 40003,
                "message": "参数错误"
            }

+ Response 404 (application/json)
    + Body

            {
                "code": 40401,
                "message": "内容已删除"
            }

## 删除子评论 [POST /`type`/comment/delete/`commentId`]


+ Request (application/json)
    + Headers

            Authorization: Bearer JWT-Token

+ Response 204 (application/json)

+ Response 400 (application/json)
    + Body

            {
                "code": 40003,
                "message": "参数错误"
            }

+ Response 404 (application/json)
    + Body

            {
                "code": 40401,
                "message": "该评论已被删除"
            }

+ Response 403 (application/json)
    + Body

            {
                "code": 40301,
                "message": "继续操作前请先登录"
            }

## 喜欢子评论 [POST /`type`/comment/toggleLike/`commentId`]


+ Request (application/json)
    + Headers

            Authorization: Bearer JWT-Token

+ Response 201 (application/json)
    + Body

            {
                "code": 0,
                "data": "是否已喜欢"
            }

+ Response 400 (application/json)
    + Body

            {
                "code": 40003,
                "message": "参数错误"
            }

# 图片相关接口

## 获取首页banner图 [GET /image/banner]


+ Response 200 (application/json)
    + Body

            {
                "code": 0,
                "data": "图片列表"
            }

## 获取 Geetest 验证码 [GET /image/captcha]


+ Response 200 (application/json)
    + Body

            {
                "code": 0,
                "data": {
                    "success": "数字0或1",
                    "gt": "Geetest.gt",
                    "challenge": "Geetest.challenge",
                    "payload": "字符串荷载"
                }
            }

## 获取图片上传token [GET /image/uptoken]


+ Request (application/json)
    + Headers

            Authorization: Bearer JWT-Token

+ Response 200 (application/json)
    + Body

            {
                "code": 0,
                "data": {
                    "upToken": "上传图片的token",
                    "expiredAt": "token过期时的时间戳，单位为s"
                }
            }

+ Response 401 (application/json)
    + Body

            {
                "code": 40104,
                "message": "未登录的用户"
            }

## 上传相册图片时，可选的 tag s和 size 列表 [GET /image/uploadType]


+ Response 200 (application/json)
    + Body

            {
                "code": 0,
                "data": {
                    "size": "可选的图片尺寸分类",
                    "tags": "可选的图片标签分类"
                }
            }

## 上传相册图片 [POST /image/upload]
> 图片对象示例：
1. `key` 七牛传图后得到的 key，不包含图片地址的 host，如一张图片 image.calibur.tv/user/1/avatar.png，七牛返回的 key 是：user/1/avatar.png，将这个 key 传到后端
2. `width` 图片的宽度，七牛上传图片后得到
3. `height` 图片的高度，七牛上传图片后得到
4. `size` 图片的尺寸，七牛上传图片后得到
5. `type` 图片的类型，七牛上传图片后得到

+ Parameters
    + bangumiId: (integer, required) - 所选的番剧 id（bangumi.id）
    + creator: (boolean, required) - 是否是原创
    + images: (array, required) - 图片对象列表
    + size: (integer, required) - 所选的尺寸 id（size.id）
    + tags: (integer, required) - 所选的标签 id（tag.id）
    + roleId: (integer, required) - 所选的角色 id（role.id）
    + albumId: (integer, required) - 所选的相册 id（album.id）

+ Request (application/json)
    + Headers

            Authorization: Bearer JWT-Token

+ Response 201 (application/json)
    + Body

            {
                "code": 0,
                "data": "图片对象列表"
            }

+ Response 400 (application/json)
    + Body

            {
                "code": 40003,
                "message": "参数错误"
            }

## 编辑图片 [POST /image/edit]


+ Parameters
    + id: (integer, required) - 图片的 id
    + bangumiId: (integer, required) - 所选的番剧 id（bangumi.id）
    + size: (integer, required) - 所选的尺寸 id（size.id）
    + tags: (integer, required) - 所选的标签 id（tag.id）
    + roleId: (integer, required) - 所选的角色 id（role.id）

+ Request (application/json)
    + Headers

            Authorization: Bearer JWT-Token

+ Response 200 (application/json)
    + Body

            {
                "code": 0,
                "data": "新的图片对象"
            }

+ Response 400 (application/json)
    + Body

            {
                "code": 40003,
                "message": "参数错误"
            }

## 删除图片 [POST /image/delete]


+ Parameters
    + id: (integer, required) - 图片的 id

+ Request (application/json)
    + Headers

            Authorization: Bearer JWT-Token

+ Response 204 (application/json)

+ Response 400 (application/json)
    + Body

            {
                "code": 40104,
                "message": "未登录的用户"
            }

+ Response 404 (application/json)
    + Body

            {
                "code": 40401,
                "message": "不存在的图片"
            }

## 举报图片 [POST /image/report]


+ Parameters
    + id: (integer, required) - 图片的 id

+ Response 204 (application/json)

## 喜欢或取消喜欢图片 [POST /image/toggleLike]


+ Parameters
    + id: (integer, required) - 图片的 id

+ Request (application/json)
    + Headers

            Authorization: Bearer JWT-Token

+ Response 201 (application/json)
    + Body

            {
                "code": 0,
                "data": "是否已点赞"
            }

+ Response 400 (application/json)
    + Body

            {
                "code": 40104,
                "message": "未登录的用户"
            }

+ Response 403 (application/json)
    + Body

            {
                "code": 40003,
                "message": "不能为自己的图片点赞|`原创图片`金币不足"
            }

+ Response 404 (application/json)
    + Body

            {
                "code": 40401,
                "message": "不存在的图片"
            }

## 新建相册 [POST /image/createAlbum]


+ Parameters
    + bangumiId: (integer, required) - 所选的番剧 id（bangumi.id）
    + creator: (boolean, required) - 是否是原创
    + isCartoon: (boolean, required) - 是不是漫画
    + name: (string, required) - 相册名`20字以内`
    + url: (string, required) - 封面图片的 url，不包含`host`
    + width: (integer, required) - 封面图片的宽度
    + height: (integer, required) - 封面图片的高度

+ Request (application/json)
    + Headers

            Authorization: Bearer JWT-Token

+ Response 201 (application/json)
    + Body

            {
                "code": 0,
                "data": "相册对象"
            }

+ Response 400 (application/json)
    + Body

            {
                "code": 40003,
                "message": "参数错误"
            }

## 编辑相册 [POST /image/editAlbum]


+ Parameters
    + id: (integer, required) - 相册的 id
    + bangumiId: (integer, required) - 所选的番剧 id（bangumi.id）
    + name: (string, required) - 相册名`20字以内`
    + url: (string, required) - 封面图片的 url，不包含`host`
    + width: (integer, required) - 封面图片的宽度
    + height: (integer, required) - 封面图片的高度

+ Request (application/json)
    + Headers

            Authorization: Bearer JWT-Token

+ Response 200 (application/json)
    + Body

            {
                "code": 0,
                "data": "相册对象"
            }

+ Response 400 (application/json)
    + Body

            {
                "code": 40003,
                "message": "参数错误"
            }

+ Response 404 (application/json)
    + Body

            {
                "code": 40401,
                "message": "不存在的相册"
            }

## 相册详情 [GET /image/album/`albumId`/show]


+ Response 200 (application/json)
    + Body

            {
                "code": 0,
                "data": "相册信息"
            }

+ Response 404 (application/json)
    + Body

            {
                "code": 40401,
                "message": "不存在的相册"
            }

## 相册内图片的排序 [POST /image/album/`albumId`/sort]


+ Parameters
    + result: (string, required) - 相册内图片排序后的`ids`，用`,`拼接的字符串

+ Request (application/json)
    + Headers

            Authorization: Bearer JWT-Token

+ Response 204 (application/json)

+ Response 400 (application/json)
    + Body

            {
                "code": 40003,
                "data": "参数错误"
            }

+ Response 404 (application/json)
    + Body

            {
                "code": 40401,
                "message": "不存在的相册"
            }

## 删除相册里的图片 [POST /image/album/`albumId`/deleteImage]


+ Parameters
    + result: (string, required) - 相册内图片排序后的`ids`，用`,`拼接的字符串

+ Request (application/json)
    + Headers

            Authorization: Bearer JWT-Token

+ Response 204 (application/json)

+ Response 400 (application/json)
    + Body

            {
                "code": 40003,
                "data": "至少要保留一张图"
            }

+ Response 404 (application/json)
    + Body

            {
                "code": 40401,
                "message": "不存在的相册"
            }

## 图片查看大图时的标记 [POST /image/viewedMark]


+ Parameters
    + id: (integer, required) - 图片的 id

+ Response 204 (application/json)

# 排行相关接口

## 动漫角色排行榜 [GET /trending/cartoon_role]


+ Parameters
    + seenIds: (string, required) - 看过的帖子的`ids`, 用','号分割的字符串

+ Response 200 (application/json)
    + Body

            {
                "code": 0,
                "0": {
                    "data": "角色列表"
                }
            }