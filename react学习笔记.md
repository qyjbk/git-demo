#### 标签引入react

准备一个容器写上id属性

首先引入react的核心库``react.development.js``，全局会多一个React对象

引入react操作dom的插件库``react-dom.development.js``，全局会多一个ReactDom对象

引入babel，将jsx转换为js``babel.min.js ``

script的标签需要声明type="text/babel",写的虚拟dom不加引号

```javascript
const VDom = <h1>hello,React!</p>
```

使用原生js写虚拟dom

```javascript
const VDom = React.createElement("h1",{id:'demo'},"hello,React!")
```

将虚拟dom渲染到页面上

```javascript
ReactDOM.render(VDom, document.getElementById('test'))
```

#### 虚拟dom本质上是一个对象

#### jsx语法规则

定义虚拟dom不需要加引号

标签中混入js表达式时要用{}

样式中的类名指定不要用class，要用className。

内联样式要用```style={{color: red,}}```

虚拟dom只能有一个根标签

标签必须闭合

##### 标签首字母

 react自定义组件标签首字母需要大写

小写字母开头转为html同名元素，如果html中没有则报错

#### 函数式组件

```javascript
function MyComponent(){
	return <h1>hello component!</h1>
}
```

```
ReactDOM.render(<MyComponent/>, document.getElementById('app'))
```

#### 类式组件

```
class MyComponent extend React.Component {
	render(){
		return(<div><h1>hello,cm</h1></div>)
	}
}
```

#### 组件的状态（state）

```
class MyComponent extend React.Component {
	constructor(props){
		super(props)
		this.state = {ishot: true}
	}
	render(){
		return(<div><h1>hello,cm</h1></div>)
	}
}
```

此时组件的实例的state都会为``{isHot: true}``

#### react事件绑定

```html
<script type='text/babel'>
	class Weather extends React.Component{
		render(){
			return <h1 onClick={demo()}></h1>
		}
	}
	function demo(){
		console('succ')
	}
</script>
```

其中的this指向会出现问题

要么使用静态that封装

```
<script type='text/babel'>
	class Weather extends React.Component{
        constructor(props){
            super(props)
            this.state = {ishot: true}
            that = this
        }
		render(){
			return <h1 onClick={demo}></h1>
		}
	}
	function demo(){
		console(this)
	}
</script>
```

将函数封装在类中，即函数为类的实例的方法

```
<script type='text/babel'>
	class Weather extends React.Component{
        constructor(props){
            super(props)
            this.state = {ishot: true}
            that = this
         }
		render(){
			return <h1 onClick={this.demo}></h1>
		}
         function demo(){
            console(this)
 	     }
	}

</script>
```

而onclick是将函数的地址返回给了html元素，所以调用时会导致this丢失

### 构造函数的用法

构造函数时用于动态写入实例上的方法和属性的

#### 在<u>构造函数</u>中使用bind解决this指向问题

this.changeWeather = this.changeWeather.bind(this)

#### 在构造函数中写入状态

this.state = {}





#### react状态不可直接更改

通过this.setState({isHot: !isHot})修改，这种方法是合并，非更新

#### state的简写形式

```javascript
class Weather extend React.Component{
    // ES6类的语法直接在写在实例身上的属性
    state = {isHot: true}
    // 将自定义方法写在实例上
    // 便于方法中箭头函数的this指向实例
    doSomeThing = () => {}
    
}
```

#### 注意展开运算符不能展开一个对象

#### {...obj}字面量形式复制一个对象

babel+react可以在VDOM里面展开一个对象，但不可滥用

### props

##### 正常写法

```
<student name='sis' age={18}/>
// 默认传入的是字符串，想要传入的是数字需要用js写法
```

此时在实例的render上调用this.props.name

##### 对象写法

```
<student {...p}/>
```

#### 对传入的props做限制

需要引入prop-type.js作为插件，16版本后

##### 写法

```
// 对类型及必要性做限制
Person.propTypes = {
	name: PropTypes.string.Require,
	sex: PropTypes.string,
	age: PropTypes.number,
	sayHi: PropType.func, // 限制类型为函数
}
// 对默认值做限制
Person.propTypes = {
	age: 18,
	sex: "未知"
}
```

在ES6中使用static这种简写方法

```
static propTypes = {}
static defaultProps = {}
```

#### 函数式组件可以拿到props

```
function MyComponent(props){
	return <p>{props.name}</p>
}

<MyComponent name='sisous'/>
```

#### 函数式组件只能在原型上写prpo约束条件

```
MyComponent.propTypes = {}
MyComponent.defaultProps = {}
```





### ref

#### 普通写法

```
<p ref='demop'>hello ref</p>
```

通过this.refs.demop

普通写法存在效率问题，已被舍弃

#### 回调函数写法

```
<p ref={ cn => this.demop = cn }
```

直接在vm身上写了一个属性demop值为ref标记的节点

##### 回调ref执行次数

内联回调函数写法的ref在更新过程时会执行两次，第一次为了确保上次的节点被清除会传入null用于清除上次的节点，第二次才会传入更新后的当前节点

直接用回调函数的写法是无关紧要的，支持直接使用

#### 将回调函数定义成class绑定的方式

```
<p ref={dosomething()}>hello ref.class</p>
```

可以避免在更新过程中ref重新被调用

#### React.createRef

在类中写入实例属性

```
myRef = React.createRef()
```

在打标点元素节点中写入

```
<p ref={this.myRef} > hello ref.class </p>
```



### jsx注释方法

```
{/* dosomething */}
```

#### 非受控组件

指的是通过ref来绑定，当需要获取数据是将取出被ref标记的节点

#### 受控组件

指的是通过onChange函数来监听节点，当节点的数据改变时同时改变state的数据，类似于vue的双向绑定，最终取数据通过state



### 柯里化函数实现回调函数通用化



### 卸载组件的函数

```
React.unmountComponentWillMount(document.getElementById('app'))
```



### React生命周期（旧）

把控制state的定时函数设置在render上时，会导致每次调用这个定时函数都再次执行render从而又新开一个定时函数，导致无限调用定时函数

#### componentDidMount

组件挂载页面完成之后调用

#### render

初始化渲染和状态更新之后调用

#### componentWillUnmount

 组件即将卸载时调用

#### shouldComponentUpdate

当调用setState时调用这个函数，这个函数返回true时才执行后续操作

#### componentWillUpdate

组件将要更新时调用

#### conponenyDidUpdate

组件已经更新时调用

#### forceUpdate()

强制更新，不走判断是否更新流程

#### componentWillReceiveProps

第一次接收props时不调用，在组件接收新的props时调用