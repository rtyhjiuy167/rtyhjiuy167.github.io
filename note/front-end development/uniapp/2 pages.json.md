## 配置文件

`src/pages.json`

### 局部导航配置

```json
{
    "pages": [ 
        {
            "path": "pages/index/index",
            "style": {
                "navigationBarTitleText": "uni-app", // 页面导航栏标题
                "navigationBarBackgroundColor": "#666", // 页面导航栏背景色
                "navigationBarTextStyle": "black", // 页面导航栏字体颜色 black 或 white
                "navigationStyle": "custom", // 导航是否打开 default打开 custom 关闭
                "backgroundColor": "#666",// 背景设置
                "enablePullDownRefresh": true // 下拉刷新设置
            }
        }
    ]
}
```

### 全部配置

```json
{
	"globalStyle": {
		"navigationBarTextStyle": "black",
		"navigationBarTitleText": "uni-app",
		"navigationBarBackgroundColor": "#F8F8F8",
		"backgroundColor": "#F8F8F8" 
	}	
}
```

### 底部导航配置

```json
{
	"tabBar": {
		"list": [
			{
				"text": "一",
				"pagePath": "pages/home/home",
				"iconPath": "static/logo.png",
				"selectedIconPath": "static/logo.png"
			},
			{
				"text": "二",
				"pagePath": "pages/index/index",
				"iconPath": "static/logo.png",
				"selectedIconPath": "static/logo.png"
			}
		]
	}
}
```

