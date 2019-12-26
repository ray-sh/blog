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