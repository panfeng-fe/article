# Nuxt.js 学习之路  

# 第一章、Nuxt简介与环境搭建

## 1、Nuxt初识

还是简单介绍下Nuxt.js吧，相信大家对vue.js耳熟能详了吧，但是vue的缺点大家也多少知道点。

### 缺点一：

单页面，使得前后端分离，导致SEO检索做的很差。虽然谷歌可以抓取SPA应用，但是百度还远远做不到这一步，所以一些需要搜索引擎抓取的页面使用vue构建是非常不友好的。

### 缺点二：

单页面工程是将所有页面和相关资源打包到一起，这就导致首屏加载时间很长，一定程度影响用户体验；这个缺点就没有上面的缺点致命，应为可以通过Nginx服务器配置Gzip压缩，配置得当可以缩小一般的文件体积是完全OK的，另外可以用CDN加速、OSS云对象存储服务器都是不错的方法。

Nuxt解决vue两大缺点，致使Nuxt在前端SSR框架占有一席之地。前端框架三兄弟如下：

Vue.js 		  ---->    Nuxt.jse

React.js 		---->    Next.js

Angular.js     ---->    Nest.js

大家是不是被搞晕了，哈哈



## 2、 Nuxt的安装

首先请确保你的npm版本不要过于怀旧，并安装了npx（npx在NPM版本5.2.0默认安装了）

```js
$ npx create-nuxt-app <项目名>
```

或者用yarn（确实很好用） ：

```js
$ yarn create nuxt-app <项目名>
```

在安装过程中会有一堆提问项需要你选择，按照你学习的需求或者项目的需求选择；我建议大家学习的时候就打开esLint，学会编写规范的代码。不要成为野生程序员。



当执行完成没有报错时就安装成功了，打开package.json文件夹，看看问价依赖和启动命令，按照启动命令在命令行输入就可以启动了，将命令行地址在浏览器打开就可以看到页面了



# 第二章、博客框架的搭建（1）

## 1、头部组件的编写

```
@components/header.vue
<template>
    <div class="header">
        <el-row type="flex" justify="center">
            <el-col :span="6">
                <span class="header-logo">Pain</span>
                <span class="header-txt">Web全栈开发工程师.</span>
            </el-col>

            <el-col :span="12">                
                <el-menu
                    :router="true"
                    :default-active="$route.name"
                    class="el-menu-demo"
                    mode="horizontal"
                    active-text-color="#1e90ff">
                    <el-menu-item :route="{name: 'about'}" index="about">
                        <template slot="title">
                            <span>关于</span>
                        </template>
                    </el-menu-item>
                    <el-menu-item :route="{name: 'index'}" index="index">
                        <template slot="title">
                            <span>首页</span>
                        </template>
                    </el-menu-item>
                </el-menu>
            </el-col>
        </el-row>
    </div>
</template>
<style src="@/assets/style/components/header.css"></style>

@asstes/style/components/header.css
.header{
    background-color: #fff;
    padding: .4rem;
    margin-bottom: .4rem;
    overflow: hidden;
    height: 4rem;
    border-bottom:1px solid #eee;
   
}
.header-logo{
    color:#1e90ff;
    font-size: 1.4rem;
    line-height: 4rem;
    text-align: center;
}
.header-txt{
    font-size: 1rem;
    color: #999;
    display: inline-block;
    padding-left: 0.3rem;
}

.el-menu-item{
    border-bottom: none!important;
    font-size: 1rem;
    font-weight: 500;
    float: right!important;
    padding: 0 1rem!important;
}
.el-menu{
    border-bottom: none!important;
    height: 3rem;
}

头部组件都是些基础我就不加以赘述了，有疑问的可以在关于找到群号，加群讨论。
```



## 2、footer组件的编写

```
@components/footer.vue
<template>
    <footer>
        <el-row type="flex" justify="center">
            <el-col :span="24" class="footer">
                <p>2020 Pain BLOG By 潘峰，本站博客未经授权禁止转载。</p>
                <p>系统有Nuxt.js + iris(Golang)构建</p>
                <p>Copyright © 皖ICP备19002392号-1</p>
            </el-col>
        </el-row>
    </footer>
</template>
<style src="@/assets/style/components/footer.css"></style>

@asstes/style/components/footer.css
.footer{
    font-size: 0.9rem;
    color: gray;
    text-align: center;
    line-height: 1.5rem;
    background-color: #fff;
    overflow: hidden;
    padding: .4rem;
    border-top:1px solid #eee;
    font-family: Arial, Helvetica, sans-serif;
  }
footer组件就更简单了
```



## 3、侧边栏side组件的编写

```
@components/side.vue
<template>
  <div class="side">
    <p>
      <el-tag type="success" @click="btnFlashAticleFind">文章分类（flash）</el-tag>
    </p>
    <el-row type="flex"
            class="side-articlelist"
            v-for="(item,index) in info"
            :key="index">
          <el-tag :type=" isActive == item.Kind ? 'danger':'info'"
              class="side-articlelist-item"
              @click="btnArticleList(item)">
              {{item.KindName}}({{item.Count}})
              </el-tag>
    </el-row>
  </div>
</template>

<script>
import { getArticleType } from "@/api/api"
import { Message } from 'element-ui'
export default {
  data () {
    return {
      type: "danger",
      info: [],
      isActive: -1,
    }
  },

  fetch () {
    return getArticleType().then((res) => {
      if (res.data.state === true) {
        this.info = res.data.data
      } else {
        Message({
          message: res.data.msg,
          type: 'error',
          duration: 3 * 1000
        })
      }
    }).catch((err) => console.log(err));
  },

  methods: {
    btnArticleList: function (item) {
      this.isActive = item.Kind
      this.$router.push({name:'articlelist-kind',params:{kind:item.Kind}})
    },

    btnFlashAticleFind: function(){
      return getArticleType().then((res) => {
        if (res.data.state === true) {
          this.info = res.data.data
        } else {
          Message({
            message: res.data.msg,
            type: 'error',
            duration: 3 * 1000
          })
        }
      }).catch((err) => console.log(err));
    }
  }
}
</script>
<style src="@/assets/style/components/side.css"></style>

@asstes/style/components/side.css
.side{
    background-color: #fff;
    padding: .4rem;
    overflow: hidden;
    border-bottom:1px solid #eee;
    /* float: right; */
}

.side-articlelist{
    flex-direction: column;
    
}
.side-articlelist-item{
    text-align: center;
    margin-top: .4rem;
    font-size: 0.5rem;
}

1、组件作用
组件被挂载时向数据接口发送axios请求，请求所有文章的种类，文章的篇数。以列表的形式显示在右侧，

2、点击跳转文章列表页，并激活当前所在种类的样式
```

**tips:nuxt的生命周期函数与vue有些不同，渲染时获取数据的方法不在时像Vue的 create()，mounted()**
**而是asyncDate()与fetch(),在page下面的的页面需要使用asyncDate()，组件则使用fetch()，**
**如果不这样写时获取不到数据的，我好不容易从坑里爬出来，你们别掉进去了**



**tips：在nuxt中从xxx/articlelist/2跳转到xxxx/articlelist/1认为你路由变化了，所以会发送数据请求；**

**从xxxx/articlelist?kind=2跳转到xxxx/articlelist?kind=1认为你路由没变化，所以不会发送数据请求.**



## 4、默认模板样式

```
<template>
    <div>
        <Header/>
        <el-row type="flex" justify="center" class="comm-container">
          <el-col :span="14">
            <nuxt />
          </el-col>
          <el-col :span="4">
            <Side/>
          </el-col>
        </el-row>
        <Footer/>
    </div>
</template>

<script>
import Header from '@/components/header'
import Footer from '@/components/footer'
import Side from '@/components/side'
export default {
  components:{
    Header,
    Footer,
    Side
  },
}
</script>


<style>
html {
  font-family: 'Source Sans Pro', -apple-system, BlinkMacSystemFont, 'Segoe UI',
    Roboto, 'Helvetica Neue', Arial, sans-serif;
  font-size: 16px;
  word-spacing: 1px;
  -ms-text-size-adjust: 100%;
  -webkit-text-size-adjust: 100%;
  -moz-osx-font-smoothing: grayscale;
  -webkit-font-smoothing: antialiased;
  box-sizing: border-box;
}

*,
*:before,
*:after {
  box-sizing: border-box;
  margin: 0;
}
</style>

将这些不则么变划的组件写在default.vue里会有很多好处
1、当页面跳转时，只有<nuxt />这部分会发生变化，这里的代码会发生重排，其他组件不会发生变化，节约渲染升本。
2、side组件里有数据请求，如果写在page的页面里，每次发生跳转都会向接口请求冗余数据十分没有必要
```

## 第二章、博客框架的搭建（2）

## 5、文章首页

```
@pages/index.vue
<template>
  <div style="box-sizing:border-box;margin: 0.4rem;">
    <div v-for="(item,index) in articleList"
         :key="index">
      <el-card class="article">
        <div slot="header"
             class="clearfix">
          <span>{{item.Title}}</span>
          <nuxt-link style="float: right; padding: 3px 0"
                     :to="{name:'articledetail-id',query:{id:item.ID}}">阅读文章</nuxt-link>
        </div>
        <div>
          <el-tag type="info"
                  size="small">Author : {{item.Author}}</el-tag>
          <el-tag type="warning"
                  size="small">Time : {{item.AddTime}}</el-tag>
          <el-tag type="danger"
                  size="small">Watch : {{item.Watch}}</el-tag>
          <el-tag type="danger"
                  size="small">Kind : {{item.KindName}}</el-tag>
        </div>
        <div class="article-introduce">{{item.Introduce}}</div>
      </el-card>
    </div>
  </div>
</template>

<script>
import { getArticleList } from "@/api/api"
import { Message } from 'element-ui'
export default {
  data () {
    return {
      articleList: [],
    }
  },

  asyncData () {
    return getArticleList({page:1,size:15}).then(res => {
      if (res.data.state === true) {
        return { articleList: res.data.data }
      } else {
        Message({
          message: res.data.msg,
          type: 'error',
          duration: 3 * 1000
        })
      }
    }).catch(err => console.log(err));
  },

}
</script>
<style src="@/assets/style/pages/index.css"></style>

@asstes/style/pages/index.css
.article{
    margin: 0.4rem 0.4rem 1rem 0.4rem;
}

.article-introduce{
    margin-top: 1rem;
}

.clearfix:before,
.clearfix:after {
  display: table;
  content: "";
}
.clearfix:after {
  clear: both
}

文章首页没有什么太多可说的，都是些vue的基础
```

**tips:还是asyncDate()与Fetch()的小坑，在asyncDate()中需要这样写**

**return { articleList: data }**

**而Fetch()中则是**

**this.articleList = res.data**

**注意区分，否则拿不到数据**



## 6、文章列表页

```
@pages/articlelist/_id.vue
<template>
  <div style="box-sizing:border-box;">
    <div v-for="(item,index) in articleList"
         :key="index">
      <el-card class="article">
        <div slot="header"
             class="clearfix">
          <span>{{item.Title}}</span>
          <nuxt-link style="float: right; padding: 3px 0"
                     :to="{name:'articledetail-id',query:{id:item.ID,page:1,size:15}}">阅读文章</nuxt-link>
        </div>
        <div>
          <el-tag type="info"
                  size="small">Author : {{item.Author}}</el-tag>
          <el-tag type="warning"
                  size="small">Time : {{item.AddTime}}</el-tag>
          <el-tag type="danger"
                  size="small">Watch : {{item.Watch}}</el-tag>
          <el-tag type="danger"
                  size="small">Kind : {{item.KindName}}</el-tag>
        </div>
        <div class="article-introduce">{{item.Introduce}}</div>
      </el-card>
    </div>
  </div>
</template>

<script>
import { getArticleByKind } from "@/api/api"
import { Message } from 'element-ui'
export default {
  data () {
    return {
      articleList: [],
    }
  },

  asyncData ({ params }) {
    return getArticleByKind(params.kind).then(res => {
      if (res.data.state === true) {
        return { articleList: res.data.data }
      } else {
        Message({
          message: res.data.msg,
          type: 'error',
          duration: 3 * 1000
        })
      }
    }).catch(err => console.log(err));
  },
}
</script>
<style src="@/assets/style/pages/index.css"></style>
```

**tips：当页面涉及路由传参时，先辨别是query传参，还是params传参，具体表现形式如：**

**xxx/article/1	params传参**

**xxx/article?id=1	query传参**

**在asyncDate()与Fetch()中接收对应的参数做axios请求**



## 7、文章详情页

```
<template>
  <div style="box-sizing:border-box;">
    <mavon-editor v-model="article.Context"
                  :subfield="false"
                  defaultOpen="preview"
                  :toolbarsFlag="false"
                  :boxShadow="false"
                  :ishljs="true" />
  </div>
</template>

<script>
import { getArticleByID } from "@/api/api"
import { Message } from 'element-ui'
export default {
  data () {
    return {
      article: {},
    }
  },

  asyncData ({ query }) {
    return getArticleByID(query.id).then(res => {
      if (res.data.state === true) {
        return { article: res.data.data }
      } else {
        Message({
          message: res.data.msg,
          type: 'error',
          duration: 3 * 1000
        })
      }
    }).catch(err => console.log(err));
  },
}
</script>

文章详情页做的事并不多，一共就两件；
1、获取数据
2、将数据以markdown样式显示数来
数据获取和前面一样，没什么可说的。markdown显示是用到一个mavon-editor的插件
安装 yarn add mavon-editor -s

注册 @plugin/vue-mavon-editor.js
import Vue from 'vue';
import mavonEditor from 'mavon-editor';
import 'mavon-editor/dist/css/index.css';
Vue.use(mavonEditor);

使用见上面代码
```



## 8、关于

```
<template>
  <div style="box-sizing:border-box;margin: 0.4rem;">
      <client-only>
       <mavon-editor v-model="Describe" :subfield="false" defaultOpen="preview"
	    :toolbarsFlag="false" :boxShadow="false" :ishljs = "true"/>
     </client-only>
  </div>
</template>
<script>
import { getAbout } from "@/api/api"

export default {
  data () {
    return {
        Describe:`博主简介`,
    }
  },

//   asyncData () {
//     return getAbout().then(res => {
//       console.log(res.data.data)
//       return { Describe: res.data.data.Describe }
//     }).catch(err => console.log(err));
//   },
}
</script>

这里的Describe可以写死，也可以在数据库获取，看你自己的需求
```



至此，博客的编写差不多都完结了，后面将写后台api的编写。

