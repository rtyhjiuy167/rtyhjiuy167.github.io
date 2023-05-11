安装gin库：`go get -u github.com/gin-gonic/gin`

```go
# 如果导包时报错，在当前目录执行下列命令
go mod init gin
go mod edit -require github.com/gin-gonic/gin@latest
go mod vendor
package main

import (
	"github.com/gin-gonic/gin"
)

func sayHello(c *gin.Context) {
	c.JSON(200, gin.H{
		"message": "hello golang!",
	})
}

func main() {
	r := gin.Default() // 创建一个默认的路由引擎

	r.GET("/hello", sayHello) // GET：请求方式；/hello：请求的路径

	//启动服务
	r.Run(":9090") //http://127.0.0.1:9090/hello
}
```