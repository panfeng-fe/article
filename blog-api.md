# 前台接口

## 基础URL

baseURL = `http://111.229.249.89:8082/blog`

## 获取文章类别

```
url：`/getArticleType`
method: 'get'

res:{
  data: [
    {
      ID: 3,
      KindName: 'Golang',
      Count: 1
    },
    {
      ID: 1,
      KindName: 'Java',
      Count: 1
    },
    {
      ID: 2,
      KindName: 'JavaScript',
      Count: 2
    },
    {
      ID: 4,
      KindName: 'Nginx',
      Count: 1
    }
  ]
```



## 获取文章列表

```
url: `/articleList?page=${page}&size=${size}`,   // default page = 1 size = 15
method: 'get'
res:  {
  data: [
    {  
      ID: 2,
      Title: 'JavaScript',
      Introduce: 'JavaScript info',
      Author: 'pain',
      Context: '## JavaScript 666666666666',
      AddTime: '2020-04-17T23:44:31+08:00',
      Watch: 5,
      Kind: 2,
      Remove: 0,
      KindName: 'JavaScript',
      IsTop: 1
    }
  ],
  msg: '获取数据成功',
  state: true
}
```



## 根据文章ID获取文章详情

```
url: `/articleByID?id=${id}`,
method: 'get'
{
    "data": {
        "ID": 1,
        "Title": "JavaScript",
        "Introduce": "9种定义js函数的方法",
        "Author": "pain",
        "Context": "**************************",
        "AddTime": "2020-04-25T23:43:42+08:00",
        "Watch": 25,
        "Kind": 2,
        "Remove": 0,
        "KindName": "JavaScript",
        "IsTop": 1
    },
    "msg": "获取数据成功",
    "state": true
}
```



## 根据文章类型获取文章列表

```
url:`/articleByKind?page=${page}&size=${size}&kind=${kind}`,
method: 'get'
{
    "data": [
        {
            "ID": 1,
            "Title": "JavaScript",
            "Introduce": "9种定义js函数的方法",
            "Author": "pain",
            "Context": "",
            "AddTime": "2020-04-25T23:43:42+08:00",
            "Watch": 26,
            "Kind": 2,
            "Remove": 0,
            "KindName": "JavaScript",
            "IsTop": 1
        },
        {
            "ID": 5,
            "Title": "Nuxt",
            "Introduce": "nuxt从入门到博客开发",
            "Author": "pain",
            "Context": "",
            "AddTime": "2020-04-21T19:34:20+08:00",
            "Watch": 28,
            "Kind": 2,
            "Remove": 0,
            "KindName": "JavaScript",
            "IsTop": 1
        }
    ],
    "msg": "获取数据成功",
    "state": true
}
```



## 提交评论

```
url:`/postComment`,
method: 'post'
data:{
	name:xxx，		// 可传可不传，传是用户，不传是游客
	articleID：1，	// 那一篇文章
	context：'xxxx'	// 类容
}
```



## 获取评论

```
url:`/commentByArticleID?articleID=1`
method: 'get'

{
    "data": [
        {
            "ID": 1,
            "ArticleID": 1,
            "Name": "pf",
            "Context": "213213131",
            "Time": "2020-05-04T22:25:11+08:00",
            "Remove": 0
        },
        {
            "ID": 2,
            "ArticleID": 1,
            "Name": "pf",
            "Context": "213213131",
            "Time": "2020-05-04T22:27:40+08:00",
            "Remove": 0
        },
        {
            "ID": 3,
            "ArticleID": 1,
            "Name": "游客",
            "Context": "213213131",
            "Time": "2020-05-04T22:28:47+08:00",
            "Remove": 0
        }
    ],
    "msg": "获取数据成功",
    "state": true
}
```



# 后台接口

## 基础URL

baseURL = `http://111.229.249.89:8082/admin`

还在写。。。