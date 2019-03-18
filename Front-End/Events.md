- 事件的名都是小驼峰写法
- 参数是一个函数
- 不能用false阻止一个事件

```js
//html
<a href="#" onclick="console.log('The link was clicked.'); return false">
  Click me
</a>


//react
function ActionLink() {
  function handleClick(e) {         //e是一个合成事件(synthetic event)
    e.preventDefault();
    console.log('The link was clicked.');
  }

  return (
    <a href="#" onClick={handleClick}>
      Click me
    </a>
  );
}
```

记得要绑定this
就是如果函数没有()，那就应该绑定，这是js的一部分，去看js
或者使用属性初始化或回调函数的方法，但回调函数会创建额外的回调函数，并且影响低阶组件的渲染，尽量不要使用

```js

  
////////////////////////////////////
//optional-1



//option-2  
class LoggingButton extends React.Component {
  handleClick() {
    console.log('this is:', this);
  }

  render() {
    // This syntax ensures `this` is bound within handleClick
    return (
      <button onClick={(e) => this.handleClick(e)}>
        Click me
      </button>
    );
  }
}
  
ReactDOM.render(
    <Toggle />,
    document.getElementById('root')
);
  
 
```

向事件传递参数

```js
//对事件传参有两种方式
    //箭头函数
   //绑定

//箭头函数事件对象必须显示传递
//绑定的话，是隐式传递





//绑定传参，事件对象e要放在最后

```

绑定this有四种方法
1. 构造函数绑定

```js
class Toggle extends React.Component {
    constructor(props) {
      super(props);
      this.state = {isToggleOn: true};
  
      // This binding is necessary to make `this` work in the callback
      this.handleClick = this.handleClick.bind(this);
    }
  
    handleClick() {
      this.setState(state => ({
        isToggleOn: !state.isToggleOn
      }));
    }
  
    render() {
      return (
        <button onClick={this.handleClick}>
          {this.state.isToggleOn ? 'ON' : 'OFF'}
        </button>
      );
    }
}
```

2. 属性初始化绑定（class field syntax）

```js
class LoggingButton extends React.Component {
    // This syntax ensures `this` is bound within handleClick.
    // Warning: this is *experimental* syntax.
    handleClick = () => {
      console.log('this is:', this);
    }
  
    render() {
      return (
        <button onClick={this.handleClick}>
          Click me
        </button>
      );
    }
}
```

3. 箭头函数     //显示传参      Parameters

```js
class LoggingButton extends React.Component {
  handleClick() {
    console.log('this is:', this);
  }

  render() {
    // This syntax ensures `this` is bound within handleClick
    return (
      <button onClick={(e) => this.handleClick(e)}>
        Click me
      </button>
    );
  }
}
<button onClick={(e) => this.deleteRow(id, e)}>Delete Row</button>  
```

4. bind绑定     //隐式传参      Arguments

```js
class Popper extends React.Component{
    constructor(){
        super();
        this.state = {name:'Hello world!'};
    }
    
    preventPop(name, e){    //事件对象e要放在最后
        e.preventDefault();
        alert(name);
    }
    
    render(){
        return (
            <div>
                <p>hello</p>
                {/* Pass params via bind() method. */}
                <a href="https://reactjs.org" onClick={this.preventPop.bind(this,this.state.name)}>Click</a>
            </div>
        );
    }
}

<button onClick={this.deleteRow.bind(this, id)}>Delete Row</button>  
```

其中3和4都会生成额外的回调函数，影响性能。一般用1或2，但是如果要传递额外的参数，就只能用3，4。同时用4的话，事件对象e要放在最后。