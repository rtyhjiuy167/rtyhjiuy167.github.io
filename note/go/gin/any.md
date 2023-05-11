```go
package main

import (
	"net/http"

	"github.com/gin-gonic/gin"
)

func main() {
	r := gin.Default()

	//使用any 简化对同一路由的不同请求方式
	r.Any("/user", func(c *gin.Context) {
		switch c.Request.Method {
		case http.MethodGet:
			c.JSON(http.StatusOK, gin.H{"method": "GET"})
		case http.MethodPost:
			c.JSON(http.StatusOK, gin.H{"method": "POST"})
		}
	})

	//为没有配置处理函数的路由添加处理程序，默认情况下它返回404代码
	r.NoRoute(func(c *gin.Context) {
		c.JSON(http.StatusOK, gin.H{"noroute": "没有此页面"})
	})
	r.Run(":9090")
}
```