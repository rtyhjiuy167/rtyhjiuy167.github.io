```go
package main

import (
	"net/http"

	"github.com/gin-gonic/gin"
)

func main() {
	r := gin.Default() // 创建一个默认的路由引擎

	//获取浏览器那边发送请求时携带的 querystring
	r.GET("/web", func(c *gin.Context) {
		a1 := c.Query("query")        //通过Query获得指定的key的参数
		a2, ok := c.GetQuery("query") //通过GetQuery获得指定的key的参数,没有指定的key，则返回的布尔值为false
		if !ok {
			a2 = "nil"
		}
		a3 := c.DefaultQuery("query", "nil") //通过DefaultQuery获得指定的key的参数,如果没有该key，则自定义赋默认值
		c.JSON(http.StatusOK, gin.H{
			"a1": a1,
			"a2": a2,
			"a3": a3,
		})
	})

	//启动服务
	r.Run(":9090")
}
```