# 引入组件库

```  react
import React from 'react';

import ReactDOM from 'react-dom';

{*两个最基本的组件库*}
```



# 创建组件的两种方式

### 函数组件 无生命周期

```javascript
function Demo(){
    function add(value) {
        console.log(value);
    }
    return(
    	<div>
        	//逻辑代码需要写在{}中
        	//利用map便利得到的列表项需要设置key值来保持唯一性，非到不得已情况下才会采用index作为key值
        	{
                [1,2,3].map((item)=>{
        			return(
                    	<li key={item} onClick={add(item)}>{item}</li>
                    )
    			})
            }
        </div>
    )
}
export default Demo;
```

### 类组件

### 函数组件的生命周期：

```react
import {React , Component} from 'react';
import 'ReactDOM' from 'react-dom';

class  Home extends Component {
    constructor(){
        super();
        this.state={
            count:0
        }
    }
	componentWillMount(){
        {*在渲染前调用*}
    }
    componentDidMount(){
        {*在第一次渲染后立即调用*}
    }
	componentDidUpdate(){
        {*在组件更新完后立即调用，首次渲染不执行*}
    }
	componentWillUnmount(){
        {*在组件从DOM中移除之前调用*}
    }
	shouldComponentUpdate(){
        {*返回一个boolean值若为true怎会渲染，否则不会渲染*}
    }
    {*在这里使用箭头函数为了解决this的指向问题*}
    {*或者在constructor()中手动绑定this的指向*}
   	add=(value)=>{
        console.log(value);
    }
    render(){
        <div>
            {*组件直接当作标签调用*}
            {*调用组件的时候可以为路由传参，可以是变量的值，也可以是一个函数用来接收子组件的返回值*}
        	<Demo count={this.state.count} add={this.add}/>
        </div>
    }
}
export default Home;
```

# 路由

***路由的实现原理***是基于**执行上下文**的

> React Router 是基于React之上的组件库，他是为了快速添加组件，并保持和URL的同步
>
> * ```BrowserRouter``` 和 ```HashRouter```	用法相同不同的是```URl```，```HashRouter```的```URL```会在路径前面添加#
>
> 例如当你通过localhost:3000/访问时，```BrowserRouter```的路径就是```/```，而```HashRouter```则是```/#/```
>
>   > ```Router``` 作为最外层的包裹层 
>   >
>   > * 参数可以有```basename``` 是用来添加基本路径的
>   >
>   > ```Switch``` 来包裹```Route``` 来避免路由的重复匹配
>   >
>   > ```Route``` 作为引入处理```URl``` 的路由 ，```Route```  还可以是双标签双标签的引入组件,但是这么引入可能会有路由参数取不到的问题
>   >
>   > * 参数有path表示当```URL``` 和path相同时 执行第二个参数component所指向的组件(**注意这里的component和类组件继承的Component大小写是不一样的**)
>   > * exact 唯一性匹配
>   >
>   > ```Link```作为路由的跳转，本质是一个a标签
>   >
>   > * 参数to类似于a标签的```href```实现路由的跳转，但是a标签的默认行为是跳转页面，to的值可以是一个字符串，也可以是一个对象
>   >
>   > ```NavLink```和```Link```一样，不同点是```NavLink```可以为链接选中时添加样式
>   >
>   > ```Redirect``` 路由重定向

``` react
import {React , Component} from 'react';
import ReactDOM from 'react-dom';
import {BrowserRouter as Router ,Link ,NavLink ,Switch,Redirect} from 'react-router-dom';
{*HashRouter 支持刷新路径重定向*}
{*import {HashRouter as Router ,Link ,Switch,Redirect} from 'react-router-dom';*}

//可以利用basename添加基本路径
//例如 路径为/add 那么添加了basename之后路径就会在前面加上basename的值 变为 /home/database/add
<Router basename="/home/databse">
    <div>
        //Link 跳转路由
    	<Link to="/home">首页</Link>
    	<Link to="/about">关于</Link>
    	<Link to="/login">登录</Link>
        //to 可以是一个对象,对象的属性可以有路径，查询字符串，状态等
    	<Link to={{
                pathname:"/hello",
                search:"?age=16",
                state:{
                    count:0
                }
            }}>Hello</Link>
        <NavLink to="/hello" activeStyle={{
                width:"200px",color:"red"
            }}>Hello</NavLink>
    </div>
    <div>
        //路由放在Switch内部，为了避免重复匹配
        //如果路由路径相同，那么在Switch内，只会匹配一次，如果没有Switch会匹配多次
        //
        <Switch>
        	//定义路由,当跳转到指定路有时执行对应组件
        	<Route exact path="/home" component={Home}/>
        	<Route path="/home/：id" component={Home}/>
        	<Route path="/about" component={About}/>
        	<Route path="/login" component={Login}/>
        	<Route path="/hello">
        		<Hello />
        	</Route>
            //重定向两种写法
            {*<Route path="/old" render={()=>{<Redirect to="/new"/>}}"/>*}
            <Redirect from="/old" to="/new"/>
        </Switch>
        
    </div>
</Router>
```

# portal

> **Portal**  提供了一种将子节点渲染到存在于父组件以外的 DOM 节点的优秀的方案。

``` react
import React, { Component } from 'react'
import Portal from './Portal'
export default class ParentPortal extends Component {
  handleClick = ()=>{
    console.log('parentportal click');
  }
  render() {
    return (
      <div onClick = {this.handleClick}>
        <Portal />
      </div>
    )
  }
}

```

``` react
import React, { Component } from 'react'
import ReactDOM from 'react-dom'
export default class Portal extends Component {
  handleClick = ()=>{
    console.log('portal click');
  }
  render() {
    return ReactDOM.createPortal(
 			<div>
   				<h1 onClick = {this.handleClick}>Portal</h1>
		    </div>,
 		document.body        
		 )
  }
}
 
```

* ```ReactDOM.createPortal()``` 方法含有两个参数 第一个参数是可渲染的**React元素**,第二个是DOM节点
* 这样的组件虽然在```ParentPortal``` 组件调用了```Portal``` 组件但是```Portal``` 并不会渲染到```ParentPortal``` 中去，而是渲染到```createPortal``` 方法的第二个参数中



# Context 执行上下文

* **使用context最大的好处就是可以避免逐层props传递参数** 

> Context
>
> 1. 生成Context，可写在一个 JS文件中，用export 导出
>
> 2. 在根组件上import Provider，并配置Provider，加上value属性
>
> 3. 在需要获取数据的组件上import Consumer，并配置Consumer，获取数据
>
>    Consumer组件里是个函数，函数的参数是传过来的value

``` react 
import React from 'react';
export let context = React.createContext();
export let context2 = React.createContext();
```

然后在外层组件中用Provider包裹起来，调用的子组件都可以取到value属性的值

如果涉及到Provider的嵌套，子组件会由里及外找到第一层Provider，也就是最近的一层Provider上的value值

``` react
import React  from 'react'
import ReactDOM from 'react-dom';
import {context，context2} from './相对路径'
ReactDOM.render(
    <context.Provider value={123}>
        <div>ddd</div>
        <context2.Provider value={456}>
             <Child />
        </context2.Provider>
	</context.Provider>, 
document.getElementById('root'));
```

在子组件中*消费* 需要用到的值得时候，可以直接调用```React.Consumer``` 也可以设置 ```contextType``` 他会返回一个由```React.createContext()``` 创建的Context对象，你可以使用```this.context``` 来*消费* 最近的Context的值

``` react
import React, { Component } from 'react'
import {context2} from './Context'
export default class Child extends Component {
  render() {
    return (
      <div>
        子组件
        <div>{this.context}</div>
        {/* <context.Consumer>
          {
            (id)=>{
              return  <div>{id}</div>
            }
          }
        </context.Consumer> */}
      </div>
    )
  }
}
Child.contextType = context2;
```



# HOC 高阶组件

* **高阶组件的目的是为了复用组件共有的逻辑**

``` react
class Music extends Component{
    constructor() {
      super();
      this.state = {
        data: []
      }
    }
    componentDidMount() {
      let url_api = 'https://api.apiopen.top/musicRankingsDetails?type=1';
      let obj = {
        method:"post"
      }
      fetch(url_api,obj)
        .then((res) => res.json())
        .then((data) => {
          this.setState({
            data: data.result
          })
        })
    }
    render(){
       <ul>
        <h1>{this.props.a}</h1>
        {
          this.props.data.map((item,index)=>
            <li key={index}>{item.title}<br/>{item.author}</li>)
        }
      </ul>
    }
  }
```

``` react
class Music extends Component{
    constructor() {
      super();
      this.state = {
        data: []
      }
    }
    componentDidMount() {
      let url_api = 'https://api.apiopen.top/musicRankingsDetails?type=1';
      let obj = {
        method:"post"
      }
      fetch(url_api,obj)
        .then((res) => res.json())
        .then((data) => {
          this.setState({
            data: data.result
          })
        })
    }
    render(){
       <ul>
        <h1>{this.props.b}</h1>
        {
          this.props.data.map((item,index)=>
            <li key={index}>{item.title}<br/>{item.author}</li>)
        }
      </ul>
    }
  }
```

​	对于以上这两段代码来说，在实际应用当中，除了需要fetch的URL和需要渲染的内容不一样外，其他的声明周期，代码逻辑都是一样的，如果这样写两个组件，那么代码的冗余度很大，会影响到效率

​	因此采用高阶组件之后这两段代码的逻辑可以共用，封装成为一个组件来处理

``` react
import React, { Component } from 'react'

let url_api = 'https://api.apiopen.top/musicRankingsDetails?type=1';
function getdisplayName(Com){
  return Com.displayName || Com.name ||"Component"
}
function hoc(Com,url_api,title){
  class Fetch extends Component{
    constructor() {
      super();
      this.state = {
        data: []
      }
    }
    componentDidMount() {
      console.log(fetch);
      let obj = {
        method:"post"
      }
      fetch(url_api,obj)
        .then((res) => res.json())
        .then((data) => {
          this.setState({
            data: data.result
          })
        })
    }
    render(){
      //this.props 获得的是调用时传入的a b等
      return <Com {...this.props} data={this.state.data}/>
    }
  }
  //为组件动态命名
  Fetch.displayName = `Fetch(${getdisplayName(Com)})`;
  return Fetch;
}
class Music extends Component{
  render(){
    return( 
      <ul>
        <h1>{this.props.a}</h1>
        {
          this.props.data.map((item,index)=>
               <li key={index}>{item.title}<br/>{item.author}</li>)
        }
      </ul>
    )
  }
}
class Music2 extends Component{
  render(){
    return(
      <ul>
        <h1>{this.props.b}</h1>
        {
          this.props.data.map((item,index)=>
               <p key={index}>{item.title}<br/>{item.author}</p>)
        }
      </ul>
    )
  }
}
let Mymusic = hoc(Music,url_api,"音乐");
let Mymusic2 = hoc(Music2,url_api,"新闻");
export default class Hoc extends Component {
  render() {
    return (
      <div>
        <Mymusic a='音乐'/>
        <Mymusic2 b='新闻'/>
      </div>
    )
  }
}

```



通过这个方式，可以动态传入props和需要读取数据的URL，然后实例化一个组件，将两个组件不同的部分分开写，相同的部分封装共用

# Hook

##### 在函数组件中像类组件一样调用状态，处理有副作用的函数，例如定时器

* ```useState```

* ```userEffect```

  ##### react-router-dom中的hook

* ```useHistory```

* ```useLocation``` 

* ```useParams```

* `useRouteMatch`



``` react
//引入
import react,{Component,useState} from 'react';
//useState用法：
let [num,setNum] = useState(defaultValue);
这里的num就相当于，类组件当中的状态，setNum就相当于类组件中的更新状态
//useEffect用法
useEffect(()=>{
    ...一些操作
},[])
useEfftect 有两个参数
第一个参数是回调函数
第二个参数是一个变量或数组 当变量或数组变化时会执行一遍回调函数
例如像定时器、fetch数据时的动态获取等这些有副作用的操作，可以通过useEffect来处理
类似类组件当中的生命周期

react-router-dom中的四个对应的就是属性当中的history、location、params、match
在函数组件中可以通过useHistory()、useLocation()、useParams()、useRouteMatch()
可以直接获取到，直接调用，也可以进行解构赋值
```



# redux

 ![img](https://upload-images.jianshu.io/upload_images/2547292-cae4b69ca467d0f3.png?imageMogr2/auto-orient/strip|imageView2/2/w/638/format/webp) 

Redux包含`ActionCreators`、`Reducers`、`Store`

Store是数据存储中心

ActionCreators作为一个媒介，将界面的数据发送给Reducers

Reducers状态的集合，接受Action然后根据Action更新状态返回给Store

+ 创建Reducers ,Reducers 中可以同时包含多个状态通过不同的函数来处理

  可以通过redux中的combineReducers来合并多个Reducers 然后导出

``` react
import {combineReducers} from 'redux';
let todos = [1,2,3];
function todo(state=todos,action){
    switch(action.type){
      case 'add_todo_item':
        return [...state,action.value];
      case 'del_todo_item':
        let newTodo = [...state];
        newTodo.splice(action.value,1);
        return newTodo;
      default:
        return state;
    }
}

let inputValue = 'todolist';
function changeValue(state=inputValue,action){
  switch(action.type){
    case 'change_input_value':
      return [...state,action.value];
    default:
      return state;
  }
}

let userInfor={
  loginname:'',
  score:0
}
function login(state=userInfor,action){
  switch(action.type){
    case 'login_success':
      return action.value;
    default:
        return state;
  }
}

//合并reducers
let reducers = combineReducers({
  todo,changeValue,login
})

export default reducers;
```



+ 创建一个Store 并将Reducers引入

``` react
import {createStore} from 'redux';
import todo from './reducer';
let store = createStore(todo);
export default store;
```

+ actions中可以单独拉出来两个文件将Action的创建过程和Action的type集中到一起

  这样的话方便统一管理，而且组件中的代码也简洁不乱



但是即使通过以上这种方法引入Redux还是存在组件繁琐的问题在派发action的过程还可以简化

# react-redux

引入react-redux，通过connect方法来简化dispatch过程

connect是一个函数可以传入参数

可以避免在每个组件中，导入store

``` react
let mapStateToProps = (state)=>{};
let mapDispatchToProps = (dispatch)=>{
  return{
    newLog:bindActionCreators(logFetch,dispatch)
  }
}
//mapStateToProps，将store转换为props，在组件中可以通过props来调用
//可以将组件内部的函数拉出来放在组件外，返回一个对象通过props调用
	//Redux 本身提供了bindActionCreators函数，来将action包装成直接可被调用的函数。
//Login 参数是将dispatch绑定到Login组件上
export default connect(mapStateToProps,mapDispatchToProps)(Login);
```



# redux-thunk

中间引入

如果不引入中间件，那么返回的action只能是一个对象，当调用了中间件之后就可以在action中编写逻辑

```react
import {createStore,applyMiddleware} from 'redux';
import todo from './reducer';
import thunk from 'redux-thunk';
let store = createStore(todo,applyMiddleware(thunk));
export default store;
```

异步式的action,在调用中间件之后返回值是一个函数

``` react
//异步请求
export const logFetch = (value) => {
  return (dispatch) => {
      fetch('https://cnodejs.org/api/v1/user/alsotang')
        .then(res => res.json())
        .then(res => {
          console.log(res);
          let userInfo = {
            type: types.LOGIN_SUCCESS,
            value: {
              loginname: res.data.loginname,
              score: res.data.score
            }
          };。
          dispatch({  
            type: types.LOGIN_SUCCESS,
            value: userInfo
          })
        })
      }
  }
```







# Fragment

``` react 
 var obj = {
     type: 'h1',
     props: {
         id: 'box',
         class: 'box',
         children: ['hello', ' world', {
             type: 'p',
             props: {
                 id: 'title',
                 class: 'title',
                 children: ['hello', ' react']
             }
         }]
     }
 }
 function render(obj, ele) {
     var {type,props} = obj;
     // 文档碎片
     var fragment = document.createDocumentFragment();
     var doc = document.createElement(type);
     for (var i in props) {
         if (i === 'class') {
             doc.setAttribute(i, props[i]);
         } else if (i === 'children') {
             for (var j = 0; j < props[i].length; ++j) {
                 if (typeof props.children[j] === 'object') {
                     render(props.children[j], doc);
                 } else {
                     var txt = document.createTextNode(props.children[j]);
                     doc.appendChild(txt);
                 }
             }
         } else {
             doc[i] = props[i];
         }
     }
     fragment.appendChild(doc);
     ele.appendChild(fragment);
 }
 render(obj, document.getElementById('root'));
```



# refs

相当于一个全局变量，是一个对象，在不同的地方给refs添加属性，然后需要用到哪个的话直接用refs就可以选到