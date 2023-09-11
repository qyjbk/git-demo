### 确定node版本号

node -v 16.17.1

### 初始化项目（vite）

npm init vite 

### 清晰项目结构

删除css等初始化内容

### 格式化网页样式（reset-css）

#### 下载依赖

```
npm i reset-css
```

#### 引用依赖

对于格式化css需要在main中引入

```
import "reser-css"
```

### 安装scss和使用

因为仅在开发环境下需要使用scss，所以安装sass在dev环境下

```
npm i save-dev sass
```

引入编写的scss文件即可

### 禁止用户拖动图片

```
-webkit-user-drag: none;
```

### 路径别名配置（@）

目前ts对@指向src是不友好的需要手动配置。

在vite.config.ts中添加配置

```
import path from 'path'

export default defineConfig({
  plugins: [react()],
  resolve: {
  	alias: {
  		"@": path.resolve(__dirname, "./src")
  	}
  }
})
```

此时path模块会报错，node中已经有了path模块，只是缺少了对ts的一些声明配置，所以安装这个库的声明配置

```
npm i -D @types/node
```

如果path模块报错尝试

```
import * as path from 'path'
```

### 提示路径别名（@）

在tsconfig.json下写入

```
{
  "compilerOptions": {
  	"baseUrl": "./",
  	"paths": {
  		"@/*": [
  			"src/*"
  		]
  	}
  }
```

### 模块化引入css(vue的scoped)

将css的文件名改为home.module.css

```
.box{
	color: red;
	p{
		margin: 10px;
	}
}
```

在组件中引入

```
import styles from "./home.module.css"

function cm1{
	return (
		<div className='styles.box'>
			<p>hello react-scoped</p>
		</div>
	)
}
```

### 安装antd和antdicon

```
npm i antd --save
```

```
npm i --save @ant-design/icons
```

引入

```
import { Button } from "antd"
```

```
import { StarOutlined } from '@ant-design/icons';
```

### 配置老版本路由

现在src下新建router/index.tsx，将app等组件包裹

// 此组件是app包裹了home组件

```
import { Routes, Route, BrowserRouter} from 'react-router-dom'
import App from '../App'
import Home from '../views/Home'

const baseRouter = () => {
	<BrowserRouter>
		<Routers>
			<Router path="/" element={<App/>} >
				<Router path="/home" element={<Home/>} ></Router>
			</Router>
		</Routers>
	</BrowserRouter>
}

export default baseRouter
```

在main中替换app组件

```
import Router from './router'
ReactDOM.createRoot(document.getElementById('root')!).render(
  <React.StrictMode>
    <Router />
  </React.StrictMode>,
)
```

在app文件中展示

```
import { Outlet } from 'react-router-dom'

function App() {
  return (
    <>
    <p>hello redux-project</p>
    <Outlet></Outlet>
    </>
  )
}
```

### 路由跳转

在父路由下写入

```
import { Link } from 'react-router-dom'

<Link to='/home'></link>
```

### 路由重定向

在router中配置

```
import { Routes, Route, BrowserRouter, Navigate } from 'react-router-dom'

<BrowserRouter>
    <Routers>
        <Router path="/" element={<App/>} >
        	<Router path="/home" element={<Navigate to='/home' />} ></Router>
            <Router path="/home" element={<Home/>} ></Router>
        </Router>
    </Routers>
</BrowserRouter>
```

### 路由表写法

在route中写入

```
import { Navigate } from 'react-router-dom'
import Home from '../views/Home'
import About from '../views/About'

const routes = [
	{
		path: '/'
		element: 
	}
]

```

