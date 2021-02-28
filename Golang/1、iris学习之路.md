# Iris学习之路（一）

# 1、Iris简介

本来想在网上拷一段简介的，找了会；发现并没有满意的简介，所以还是我来给大家写一段我自己使用的感觉吧，不知道大家有没有接触Node.js的服务端框架，如Express、koa2、Egg；Iris给我的感觉还是和他们很像的，Web框架无论什么语言都差异不大，互相借鉴也是常有的事.

iris框架的官网上说是最快的Web框架，到底是不是我就不知道了，但是它的性能和稳定性是Node框架无法比拟的，面对大型Web后端是毫不费力的,iris是用Golang语言写的，**我默认大家都是会Golang的**。（后期也会出Go的学习笔记），废话不多说  我们来看看iris的详细信息吧。



# 2、iris的目录结构

iris并没有想Beego那个样的工具直接生成目录结构

所以iris的目录结构并不死板，你可以任意给它命名；但是还是老实点，不要玩花的。

```
conf 配置文件和项目初始化目录
ctr 全名是controller，为了省事就命名为ctr，写处理每个请求的逻辑函数
log 日志目录
middleware 中间接目录
route 路由目录
model 模型层，定义数据结构体
service 写具体业务逻辑
utils 写工具函数
main.go 函数入口
go.mod 开启go moudle生成的文件，相当于js的package.json
```



# 3、conf的初始化类容

## 3.1 、配置项

每个项目都有自己的配置，这些配置通常写在配置文件下，配置文件的种类也很多，例如ini，json，xml，toml等等，你可以选在你喜欢的配置文件，我选择的是ini类型的配置文件

```
[App]
        Name = PainBlog
        Port = :8080
        Key = xxxxxx

[Mysql]
        User = root
        Password = xxxxxx
        Host = xxxxxx
        DateBase = blog
        Port  = 1433
        Dsn = %s:%s@(%s)/%s?charset=utf8&parseTime=True&loc=Local

[Redis]
        Host = xxxxxx
        Password = xxxxxx
        Port = 6379
        Dsn = %s:%d
ini配置文件的类型如上，每个配置文件都有自己的格式，需要遵循格式书写配置信息。
```



## 3.2 、定义配置结构体

```
package conf

import (
	"github.com/jinzhu/gorm"

	"github.com/garyburd/redigo/redis"
)

type App struct {
	Name string
	Port string
	Key  string
}

type Mysql struct {
	User     string
	Password string
	Host     string
	DateBase string
	Port     int
	Dsn      string
}

type Redis struct {
	Host     string
	Port     int
	Password string
	Dsn      string
}

type Conf struct {
	App
	Mysql
	Redis
}


var (
	Config Conf
	DB *gorm.DB
	RedisPool *redis.Pool
	err error
)

func init(){
	Config.Init()
	Config.Redis.Init()
	Config.Mysql.Init()
}

从配置文件读出的信息需要写入结构体中，所以要事先定义好结构体，用来存放配置信息。
```



## 3.3 、读取配置

```
package conf

import "gopkg.in/gcfg.v1"

func (C *Conf) Init() {
	if err = gcfg.ReadFileInto(C, "./conf/server.ini"); err != nil {
		panic(err)
	}
}

将配置信息写入Conf里，如果出错抛出异常。
```



## 3.4 初始化数据库

```
package conf

import (
	"fmt"
	"time"

	"github.com/jinzhu/gorm"

	"github.com/garyburd/redigo/redis"
	_ "github.com/jinzhu/gorm/dialects/mysql"
)

func (M *Mysql) Init() {
	MysqlDsn := fmt.Sprintf(M.Dsn, M.User, M.Password, M.Host, M.DateBase)
	DB, err = gorm.Open("mysql", MysqlDsn)

	if err != nil {
		panic(err)
	}

	// DB.LogMode(true)
	DB.DB().SetMaxOpenConns(5000)
	DB.DB().SetMaxIdleConns(2000)
	DB.SingularTable(true)
}

func (R *Redis) Init() {
	redisDsn := fmt.Sprintf(R.Dsn, R.Host, R.Port)
	RedisPool = &redis.Pool{
		MaxIdle:     20,
		MaxActive:   999,
		IdleTimeout: 240 * time.Second,
		Dial: func() (redis.Conn, error) {
			c, err := redis.Dial("tcp", redisDsn)
			if err != nil {
				return nil, err
			}
			if _, err := c.Do("AUTH", R.Password); err != nil {
				c.Close()
				return nil, err
			}
			return c, err
		},
		TestOnBorrow: func(c redis.Conn, t time.Time) error {
			if time.Since(t) < time.Minute {
				return nil
			}
			_, err := c.Do("PING")
			return err
		},
	}
}
日志项目用到了mysql、redis两个数据库，所以我们来初始化
```

综上，conf的工作我们就做好了

# 4、实现主函数

我们要用iris实现一个blog-server，所以日志少不了，所以我们先在utils工具库函数里写简单的打开文件函数。

```go
utils/utils.go
func NewLogFile() *os.File {
	today := time.Now().Format("2006-01-02")
	filename := "./log/" + today + ".log"
	file, err := os.OpenFile(filename, os.O_WRONLY|os.O_CREATE|os.O_APPEND, 0666)
	if err != nil {
		panic(err)
	}
	return file
}
这个函数具体功能是根据每天不懂得日期创建日志文件
```

然后在main.go使用

```
var appConf = conf.Config.App // 引入配置
func main(){
	logFile := utils.NewLogFile()
	defer logFile.Close() // 一定要记得关闭
	app := iris.New() 	
	app.Logger().SetOutput(io.MultiWriter(logFile, os.Stdout)) 
	//初始化日志，app实例上知道logger的所以现在将文件接口传进去就可以使用了
	requestLogger := logger.New(logger.Config{
		Status: true,
		IP:     true,
		Method: true,
		Path:   true,
		Query:  true,
		//MessageContextKeys: []string{"logger_message"},
		//MessageHeaderKeys:  []string{"User-Agent"},
	})
	app.Use(requestLogger) // 将日志挂载到app实例上
	
	// 跨域相关设置
	crs := cors.New(cors.Options{
		AllowedOrigins:   []string{"*"}, 
		AllowedMethods:   []string{"GET", "POST", "PUT", "DELETE", "OPTIONS"},
		AllowedHeaders:   []string{"*"},
		ExposedHeaders:   []string{"Authorization"},
		Debug:            false,
		AllowCredentials: true,
	})
	// 将crs注册到路由上
	router := app.Party("/", crs).AllowMethods(iris.MethodOptions)
	// 路由注册
	route.RouterInit(router)

	app.Run(
		iris.Addr(":8081"),
		iris.WithoutServerError(iris.ErrServerClosed),
	)
	
}
```



# 5 、路由分发

在main.go我们将跟路由router入参RouterInit()函数，现在我们在route文件夹下实现这个函数

```
package route

import (
	"blog/ctr"
	"github.com/kataras/iris/v12"
)

func RouterInit(router iris.Party) {
	BlogInit(router.Party("blog/"))
	AdminInit(router.Party("admin/"))
}

func BlogInit(BlogRoute iris.Party) {
	BlogRoute.Get("/index", ctr.Test)
}

func AdminInit(AdminRoute iris.Party) {
	AdminRoute.Get("/index", ctr.Test)
}

由于blog系统是有管理页面的，所以我们在这将跟路由分成相应的两组对应的路由组

我们在简单实现下ctr下的Test函数我们就可以简单的访问了

package ctr

import (
	"github.com/kataras/iris/v12"
)

func Test(ctx iris.Context){
	ctx.WriteString("hello 铁憨憨！")
}

现在你在postman或者浏览器地址输入栏输入 localhost:8080/blog/index 就可以访问到了。那么简单的框架我们就搭好了，后面再写业务逻辑。

tips：我定义的端口是8080  但是提示的端口是8081，点开源码发现原来的端口被+1，具体为啥我也没整明白，但是如果我就想在8080端口监听可以在
app.Run(		
	iris.Addr(":8081"),
	iris.WithoutBanner,
	iris.WithoutServerError(iris.ErrServerClosed),
）
即可。

再看源码的同时看到iris内部支持toml的配置文件，源码如下
// TOML reads Configuration from a toml-compatible document file.
// Read more about toml's implementation at:
// https://github.com/toml-lang/toml
//
//
// Accepts the absolute path of the configuration file.
// An error will be shown to the user via panic with the error message.
// Error may occur when the file doesn't exists or is not formatted correctly.
//
// Note: if the char '~' passed as "filename" then it tries to load and return
// the configuration from the $home_directory + iris.tml,
// see `WithGlobalConfiguration` for more information.
//
// Usage:
// app.Configure(iris.WithConfiguration(iris.TOML("myconfig.tml"))) or
// app.Run([iris.Runner], iris.WithConfiguration(iris.TOML("myconfig.tml"))).
func TOML(filename string) Configuration {
	c := DefaultConfiguration()

	// check for globe configuration file and use that, otherwise
	// return the default configuration if file doesn't exist.
	if filename == globalConfigurationKeyword {
		filename = homeConfigurationFilename(".tml")
		if _, err := os.Stat(filename); os.IsNotExist(err) {
			panic("default configuration file '" + filename + "' does not exist")
		}
	}

	// get the abs
	// which will try to find the 'filename' from current workind dir too.
	tomlAbsPath, err := filepath.Abs(filename)
	if err != nil {
		panic(fmt.Errorf("toml: %w", err))
	}

	// read the raw contents of the file
	data, err := ioutil.ReadFile(tomlAbsPath)
	if err != nil {
		panic(fmt.Errorf("toml :%w", err))
	}

	// put the file's contents as toml to the default configuration(c)
	if _, err := toml.Decode(string(data), &c); err != nil {
		panic(fmt.Errorf("toml :%w", err))
	}
	// Author's notes:
	// The toml's 'usual thing' for key naming is: the_config_key instead of TheConfigKey
	// but I am always prefer to use the specific programming language's syntax
	// and the original configuration name fields for external configuration files
	// so we do 'toml: "TheConfigKeySameAsTheConfigField" instead.
	return c
}

第一部分到此为止，后面会有相关业逻辑的实现。
```

