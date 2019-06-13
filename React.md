

### 我对react的理解

> react是组件，比如你的页面有三个功能快，第一块是登录，第二块是展示，第三块是反馈，方便对应三个div
> . 那么你可以写一个登录组件，这个组件里面基本元素应该是两个输入框，一个按钮，整块东西做完了，直接把该组件放在第一个div上。 这个和bootstrp的区别是bstp是样式的，意思就是你组件里面的按钮可以用到封装好的样式
>
> React本身是没有制作ajax的，对于后台连接使用单纯的fetch不习惯的朋友，也是完全可以使用jQuery的 ajax功能的，不要总听信某些领导说react中就完全不需要使用jquery什么的.
>

### 组件

#### 组件的定义

##### 1.无状态函数式组件

1.函数来定义一个组件  //而自定义的 React 类名以大写字母开头，比如 HelloMessage 不能写成 helloMessage。除此之外还需要注意组件类只能包含一个顶层标签，否则也会报错。

```react
function HelloMessage(props) {
    return <h1>Hello World!</h1>;
}
```

2.用上面定义好的组件 生成一个const

```react
const element = <HelloMessage />;
```

3.把生成的const与对应的组件搭配

```react
ReactDOM.render(
    element,
    document.getElementById('example')
);
```

-------------------------------------------

##### 2.React.createClass

是react刚开始推荐的创建组件的方式，这是ES5的原生的JavaScript来实现的React组件，其形式如下：

```react
var InputControlES5 = React.createClass({
    propTypes: {//定义传入props中的属性各种类型
        initialValue: React.PropTypes.string
    },
    defaultProps: { //组件默认的props对象
        initialValue: ''
    },
    // 设置 initial state
    getInitialState: function() {//组件相关的状态对象
        return {
            text: this.props.initialValue || 'placeholder'
        };
    },
    handleChange: function(event) {
        this.setState({ //this represents react component instance
            text: event.target.value
        });
    },
    render: function() {
        return (
            <div>
                Type something:
                <input onChange={this.handleChange} value={this.state.text} />
            </div>
        );
    }
});
InputControlES6.propTypes = {
    initialValue: React.PropTypes.string
};
InputControlES6.defaultProps = {
    initialValue: ''
};
```

##### 3.React.Component

`React.Component`是以ES6的形式来创建react的组件的，是React目前极为推荐的创建有状态组件的方式，最终会取代`React.createClass`形式；相对于 `React.createClass`可以更好实现代码复用。将上面`React.createClass`的形式改为`React.Component`形式如下：

```react
class InputControlES6 extends React.Component {
    constructor(props) {
        super(props);

        // 设置 initial state
        this.state = {
            text: props.initialValue || 'placeholder'
        };

        // ES6 类中函数必须手动绑定
        this.handleChange = this.handleChange.bind(this);
    }

    handleChange(event) {
        this.setState({
            text: event.target.value
        });
    }

    render() {
        return (
            <div>
                Type something:
                <input onChange={this.handleChange}
               value={this.state.text} />
            </div>
        );
    }
}
InputControlES6.propTypes = {
    initialValue: React.PropTypes.string
};
InputControlES6.defaultProps = {
    initialValue: ''
};
```



#### 组件参数

如果需要往组件中传递参数，使用this.props：

```react
function HelloMessage(props) {
    return <h1>Hello {props.name}!</h1>;
}

const element = <HelloMessage name="Runoob"/>;

ReactDOM.render(
    element,
    document.getElementById('example')
);
```

#### 复合组件

```react
<script type="text/babel">
function Name(props) {
	return <h1>网站名称：{props.name}</h1>;
}
function Url(props) {
	return <h1>网站地址：{props.url}</h1>;
}
function Nickname(props) {
	return <h1>网站小名：{props.nickname}</h1>;
}
function App() {
	return (
	<div>
		<Name name="菜鸟教程" />
		<Url url="http://www.runoob.com" />
		<Nickname nickname="Runoob" />
	</div>
	);
}

ReactDOM.render(
	 <App />,
	document.getElementById('example')
);
</script>
```

### 网络请求Fetch

首页面 **index.html**：

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <link rel="shortcut icon" href="%PUBLIC_URL%/favicon.ico" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <meta name="theme-color" content="#000000" />
    <link rel="manifest" href="%PUBLIC_URL%/manifest.json" />
    <link rel="stylesheet" href="https://unpkg.com/shineout/dist/theme.default.css" />
    <script crossorigin src="https://unpkg.com/shineout/dist/shineout.min.js"></script>

    <title>React App</title>
  </head>
  <body>
    <noscript>You need to enable JavaScript to run this app.</noscript>
    <input type="text" id="input_id_1">
    <div id="div1"></div>
    <div id="div2"></div>

  </body>
</html>
```

首页面的react.js **index.js**：

```react
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import ButtonComp from './AppComponent/ButtonComponet';
import InputComp from './AppComponent/InputComponet';
import FetchComponet from './AppComponent/FetchComponet'
import * as serviceWorker from './serviceWorker';


ReactDOM.render(<ButtonComp />, document.getElementById('div1'));
ReactDOM.render(<FetchComponet />, document.getElementById('div2'));
// If you want your app to work offline and load faster, you can change
// unregister() to register() below. Note this comes with some pitfalls.
// Learn more about service workers: https://bit.ly/CRA-PWA
serviceWorker.unregister();
```

**FetchComponet组件**：

```react
import React from 'react';

class FetchComponet extends React.Component{
    constructor(props){ //构造函数
        super(props);
        this.state = {
            mytext : '初始值',
        }
    }

    getData(){ //请求数据函数
        this.setState({mytext:"1234"})
        fetch(`http://127.0.0.1:8640/ReactServer/test.do?words=a`,{
            method: 'GET'
        }).then(res => res.text()).then(
            data => {
                this.setState({mytext:data})
            }
        )
    }

    componentWillMount(){
        this.getData();
    }

    render(){
    return(
        <div>
            <div>{this.state.mytext}</div>
        </div>
    );}
}

export default FetchComponet;
```

#### 注意两个问题

1.是跨域问题 

> 什么是跨域问题？<https://www.cnblogs.com/renfanzi/p/6952871.html>
>
> Python tornado如何解决跨域问题？<https://blog.csdn.net/moshowgame/article/details/85255982>

2.调用方法写在了钩子函数中，为什么呢？<https://blog.csdn.net/daxiazouyizou/article/details/79773307>

> 下面来分析一下代码，首先，在上方的“构造函数”constructor中，设置了State的初值，添加一个mytext属性，初始值为空，其他内容是标准的构造语法，可以参照React官方资料。之后，我们创建一个getData() 函数，用于实现fetch方法获取后台服务器的数据。
>
> 如上所示，fetch方法有两个传入的参数，一个是url，也就是后台api接口所在的地址，另一个是{method:'GET'}，意思是采用GET方式与后台服务器进行通信。后面的 .then( ) 方法将上面传回来的结果进行进一步的处理，将结果作为 res这个对象传入，并使用 .text() 方法使其转化为字符串类型，然后再下一个 .then( ) 方法中将上一步的返回值作为data 赋值给 state里的mytext。
>
> （Fetch的详细用法各位可以参照一下网上的资料，本文只是简单使用，不做太多介绍）
>
> 现在我们有了获取数据的函数，需要调用这个函数，这里要注意的是，我们不能直接在下方的render函数里调用getData( )，那样会造成页面死循环，由于React的特性，在render函数中，每当State被改变时就会重新渲染组件，getData( )函数中涉及到了对State的更改，所以React系统会重新去渲染页面==>加载render函数==>重新调用getData( )==>重新渲染页面==>一直死循环...
>
> 所以我们需要使用到React的生命周期函数，componentWillMount( )，将getData函数放置在其中，这个生命周期函数会在组件被渲染前调用，这个时候改变State就不会造成死循环。
>
> （关于生命周期各位可以参照React官方文档   https://doc.react-china.org/ ）
>
> 然后，我们需要将State中的数据显示出来，在render函数return的标签中如上所示将 this.state.mytext 的内容显示出来。
>
> 保存文件，用 npm start 启动项目，输入 localhost:3000 页面将会呈现如下内容：


### react-router 路由 
https://www.jianshu.com/p/d4c42e5a5dfc

### react-redux
https://www.jianshu.com/p/a735096f3eda

### 参考文章

<http://www.ruanyifeng.com/blog/2015/03/react.html> 

<https://www.runoob.com/react/react-state.html> 
