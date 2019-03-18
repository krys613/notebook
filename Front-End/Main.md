所有react组件不能改变props的值


![IMAGE](resources/BDBCDD64AA8E5AD86748E4CC17935404.jpg =488x636)

props
state           
mounting            挂载
unmounting
lifecycle method    生命周期钩子

```js
componentDidMount() {
    this.timerID = setInterval(
        () => this.tick(),
        1000
    );
}
```

