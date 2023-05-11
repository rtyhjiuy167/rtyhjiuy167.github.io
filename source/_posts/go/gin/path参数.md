```go
package main

import (
	"net/http"

	"github.com/gin-gonic/gin"
)

// 获取请求的path(URI)参数
// 使用时注意路由匹配互相发生冲突
func main() {
	r := gin.Default() // 创建一个默认的路由引擎
	r.GET("/:name/:age", func(c *gin.Context) {
		//使用 Param 获得对应的 path参数
		name := c.Param("name")
		age := c.Param("age")
		c.JSON(http.StatusOK, gin.H{
			"name": name,
			"age":  age,
		})
	})
	//启动服务
	r.Run(":9090")
}
```