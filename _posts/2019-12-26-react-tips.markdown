---
layout: post
title:
date: 2019-12-26下午 10时18分18秒 +0800
categories: React
---

- 获取输入框的值
  - 方法1，使用原生的控件

```
class Form extends React.Component {
  //创建element的引用
  const currentUser = React.createRef()
  handleSubmit = (e) => {
    //阻止页面的刷新
    e.preventDefault()
    //获取当前引用元素的值
    console.log(this.currentUser.current.value)
    //删除当前值
    this.currentUser.current.value=''
  }
	render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <input type="text" placeholder='input name' ref = {this.currentUser}/>
        <button>add card</button>
      </form>
    )
  }
}
```

  - 方法2，将该控件变成React管理的控件

```
class Form extends React.Component {
  const currentUser = React.createRef()
  handleSubmit = (e) => {
    e.preventDefault()
    console.log(this.state.name)
    this.state.name=''
  }
  state = {name: ''}
	render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <input type="text" placeholder='input name'
          onChange={(event) => {this.setState({name: event.target.value})}}/>
        <button>add card</button>
      </form>
    )
  }
}
```

- http call,使用async, await

```
handleSubmit = async (event) => {
  	event.preventDefault();
    const resp = await axios.get(`https://api.github.com/users/${this.state.userName}`)
    console.log(resp.data);
  };
```

- useEffect的用法

```
//useEffect在每次rendering以后都会调用
useEffect(() => {
  	if (secondsLeft > 0 && availableNums.length > 0) {
      //setTimeout让一个函数延时执行，但是也间接创建了一timmer
      const timerId = setTimeout(() => {
	      setSecondsLeft(secondsLeft - 1);
      }, 1000);
      //return的函数会在下一次render之前执行，用来清除某些side effect
    	return () => clearTimeout(timerId);
  	}
  });
```

- 通过修改component的key来实现app的reset

React will discard the current App instance and create a new one with a fresh state While this is a nice trick that might reduce the complexity of your app, it is important to remember that this approach makes React discard the entire component instance and DOM tree. In the above example, most of the Chat component will be redrawn anyway on activeChat change, so it might be a good enough solution. On real word applications, you should limit non-list components with keys and avoid setting it on top level ones, as it might be the cause of (hard to spot) performance issues.

- 自定义hook

自定义hook就是一个函数封装，但是他的使用也要准守一定的规则
https://zh-hans.reactjs.org/docs/hooks-custom.html

- 不要依赖状态A的更新后的值来更新状态B，React里面状态的更新是无序的，可以通过传入一个函数给setState来实现这个效果