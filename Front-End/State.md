- 不要直接改变state

```js
this.state.xxx = "xxx";     //wrong
this.setSate({xxx:"xxx"});  //correct
```

- setState()是异步的
    因为React会一次性批处理多个setState()，尽量用方法而不是对象

```js
//wrong
this.setState({
    counter: this.state.counter + this.props.increment,
});


//correct
this.setState((state, props) => ({
    counter: state.counter + props.increment
}));
this.setState(function(state, props) {
    return {
        counter: state.counter + props.increment
    };
});
```

- 合并state的更新  ？



top-down

任何状态都会被某个组件所持有，并且它们的数据和UI只能影响其下方的组件