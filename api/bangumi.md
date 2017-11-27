### [接口文档](/api/index)

#### 获取新番列表
```javascript
url = '/bangumi/news'
method = 'GET'
authHeader = false
body = {}
response = {
  code: 0,
  message: [],
  data: [
    {
      "id": 71,
      "name": "番剧标题",
      "summary": "番剧简介",
      "avatar": "https://image.calibur.tv/Fogkr5Ry94qIFVfo7ukGAirTX9VE",
      "banner": "https://image.calibur.tv/FtKehm19E6CZMryzJi509sqJXAMy",
      "count_like": "0",
      "count_score": "0.00",
      "deleted_at": null,
      "created_at": "2016-09-09 20:44:42",
      "updated_at": "2017-11-15 22:41:00",
      "alias": {
        "search": "Re:ゼロから始める異世界生活,re0,从零开始,从零开始的异世界生活"
      },
      "season": "",
      "released_at": 0,
      "released_video_id": 0,
      "published_at": 1459612800,
      "collection_id": 0,
      "others_site_video": 0,
      "released_part": 0,
      "tags": [
          {
            "id": "2",
            "name": "恋爱"
          },
          {
            "id": "3",
            "name": "治愈"
          }
          // ...more
      ]
    }
    // ...more
  ]
}
```

#### 获取番剧详情
```javascript
url = '/bangumi/${id}/show/'
method = 'GET'
authHeader = true
body = {}
response = {
  "id": 71,
  "name": "番剧标题",
  "summary": "番剧简介",
  "avatar": "https://image.calibur.tv/Fogkr5Ry94qIFVfo7ukGAirTX9VE",
  "banner": "https://image.calibur.tv/FtKehm19E6CZMryzJi509sqJXAMy",
  "count_like": "0",
  "count_score": "0.00",
  "deleted_at": null,
  "created_at": "2016-09-09 20:44:42",
  "updated_at": "2017-11-15 22:41:00",
  "alias": {
    "search": "Re:ゼロから始める異世界生活,re0,从零开始,从零开始的异世界生活"
  },
  "season": "",
  "released_at": 0,
  "released_video_id": 0,
  "published_at": 1459612800,
  "collection_id": 0,
  "others_site_video": 0,
  "released_part": 0,
  "tags": [
    {
      "id": "2",
      "name": "恋爱"
    },
    {
      "id": "3",
      "name": "治愈"
    }
    // ...more
  ],
  "videoPackage": {
    "videos": [
      {
        "id": 428,
        "url": "",
        "name": "初始的终结与结束的开始",
        "poster": "https://image.calibur.tv/bangumi/re0/poster/1.jpg",
        "bangumi_id": "71",
        "part": 1,
        "count_played": "8",
        "count_comment": "0",
        "deleted_at": null,
        "created_at": null,
        "updated_at": "2017-10-18 22:14:59",
        "resource": {
          lyric: null,
          video: {
            720: {
              src: 'bangumi/re0/video/720/1.mp4',
              useLyc: false
            }
          }
        }
      }
      // ...more
    ],
    "repeat": false
  }
}
errorResponse = {
  code: 0,
  data: '',
  message: [
    '番剧不存在'
  ]
}
```

#### 获取分类番剧
```javascript
url = '/bangumi/tags?id=${id}'
method = 'GET'
authHeader = false
body = {}
response = {
  code: 0,
  message: [],
  data: {
    tags: [
      {
        "id": 1,
        "name": "机甲"
      },
      {
        "id": 2,
        "name": "恋爱"
      }
      // ...more
    ],
    bangumis: [
      {
        "id": 8,
        "name": "番剧标题",
        "summary": "番剧简介",
        "avatar": "https://image.calibur.tv/Fqnm4-H8Kmxy9IcKT-apyuxSn4gd",
        "banner": "https://image.calibur.tv/Fuia5CyVQGbPWze23Mqmo2QLMp05",
        "count_like": "0",
        "count_score": "0.00",
        "deleted_at": null,
        "created_at": "2016-07-03 10:32:10",
        "updated_at": "2017-11-15 22:02:41",
        "alias": {
          "search": "Code Geass,叛逆的鲁鲁修,反逆的鲁鲁修,反叛的鲁路修,叛逆的鲁路修,反叛的勒鲁什,叛逆的勒鲁什,コードギアス 反逆のルルーシュ"
        },
        "season": {
          "name": [
            "R1",
            "R2"
          ],
          "part": [
            0,
            25,
            50
          ],
          "time": [
            "2006.10",
            "2008.4"
          ]
        },
        "released_at": 0,
        "released_video_id": 0,
        "published_at": 1159977600,
        "collection_id": 0,
        "others_site_video": 0,
        "released_part": 0,
        "tags": [
          {
            "id": "1",
            "name": "机甲"
          },
          {
            "id": "3",
            "name": "治愈"
          }
        ]
      }
    ]
  }
}
example = [
  '/bangumi/tags',        // 返回的 bangumis 是空数组
  '/bangumi/tags?id=1',   // 返回 tag_id = 1 的所有番剧
  '/bangumi/tags?id=1-23' // 返回 tag_id = 1 和 23 的所有番剧
]
```