> stack

### basic
基本的关系图
![IMAGE](resources/F9BFBB2338B28824D2F78C770B6C0DBC.jpg =1811x637)

包含其它平台图
![IMAGE](resources/39E7844CD7C5E1B386848632BE24735B.jpg =1801x657)

### render flow

![IMAGE](resources/A5A798D1C8CFD12E0CD8429948802FCA.jpg =400x639)

ReactDom.render()是一个接口，实际上用的是ReactMount.render()
将jsx的描述转成dom
![IMAGE](resources/7F931CD6BB2A4DD4F73C3644D8DE9E54.jpg =781x221)

![IMAGE](resources/8BC6D1727DD758F35328ACA1D15C272E.jpg =670x190)
jsx会被转换成三个组件中的一种(嵌套)
- ReactCompositeComponent
- ReactDOMComponent
- ReactDOMTextComponent