---
title: React之Router 4（十一）
date: 2017-11-20 00:06:10
tags: 
 - Router
 - react
categories: Front-End
---

一、安装
---

> `react-router-dom`暴露出`react-router`中暴露的对象与方法，因此你只需要安装并引用`react-router-dom`即可

```javascript
npm install --save react-router-dom
```

二、路由器(Router)
---

> 在你开始项目前，你需要决定你使用的路由器的类型。对于网页项目，存在`<BrowserRouter>`与`<HashRouter>`两种组件。当存在服务器来管理动态请求时，需要使用`<BrowserRouter>`组件，而`<HashRouter>`被用于静态网站。通常，我们更倾向选择`<BrowserRouter>`，但如果你的网站仅用来呈现静态文件，那么`<HashRouter>`将会是一个好选择

三、历史(History)
---

> 每个路由器都会创建一个`history`对象并用其保持追踪当前location[注1]并且在有变化时对网站进行重新渲染。这个`history`对象保证了`React Router`提供的其他组件的可用性，所以其他组件必须在`router`内部渲染。一个`React Router`组件如果向父级上追溯却找不到`router`组件，那么这个组件将无法正常工作

四、渲染<Router>
---

> 路由器组件无法接受两个及以上的子元素。基于这种限制的存在，创建一个`<App>`组件来渲染应用其余部分是一个有效的方法

```javascript
import { BrowserRouter } from 'react-router-dom'
ReactDOM.render((
  <BrowserRouter>
    <App />
  </BrowserRouter>
), document.getElementById('root'))
```

五、`<App>`
---

> 应用通过`<App>`组件定义。简化一下，我们将应用拆分成两个部分。`<Header>`组件包含网站的导航链接。`<Main>`组件则呈现其余内容

```javascript
const App = () => (
  <div>
    <Header />
    <Main />
  </div>
)
```

六、路由(Route)
---

> `<Route>`组件是`React Router`中主要的结构单元。在任意位置只要匹配了`URL`的路径名(`pathname`)你就可以创建`<Route>`元素进行渲染

**路径(Path)**

> `<Route>`接受一个数为`string`类型的`path`，该值路由匹配的路径名的类型。例如：`<Route path='/roster'/>`会匹配以`/roster`开头的路径名。在当前`path`参数与当前`location`的路径相匹配时，路由就会开始渲染`React`元素。若不匹配，路由不会进行任何操作


```javascript
<Route path='/roster'/>
// 当路径名为'/'时, path不匹配
// 当路径名为'/roster'或'/roster/2'时, path匹配
// 当你只想匹配'/roster'时，你需要使用"exact"参数
// 则路由仅匹配'/roster'而不会匹配'/roster/2'
<Route exact path='/roster'/>
```

- 注意：在匹配路由时，`React Router`只关注`location`的路径名。当`URL`如下时

```javascript
http://www.example.com/my-projects/one?extra=false
```

- `React Router`去匹配的只是`'/my-projects/one'`这一部分

**匹配路径**

> `path-to-regexp`包用来决定`route`元素的`path`参数与当前`location`是否匹配。它将路径字符串编译成正则表达式，并与当前`location`的路径名进行匹配比较

- 当路由地址匹配成功后，会创建一个含有以下属性的`match`对象：
  - `url` ：与当前`location`路径名所匹配部分
  - `path`：路由的地址
  - `isExact` ：`path` 是否等于 `pathname`
  - `params`：从`path-to-regexp`获取的路径中取出的值都被包含在这个对象中

使用[route tester](https://pshrmn.github.io/route-tester/#/)这款工具来对路由与URL进行检验

**创建你的路由**

> 可以在路由器(router)组件中的任意位置创建多个`<Route>`，但通常我们会把它们放在同一个位置。使用`<Switch>`组件来包裹一组`<Route>`。`<Switch>`会遍历自身的子元素（即路由）并对第一个匹配当前路径的元素进行渲染

- 我们希望匹配一下路径
  - `/`：主页
  - `/roster`： 团体列表
  - `/roster/:number`：运动员页面，使用运动员的编号作为标识
  - `/schedule`：团队的赛程表
  
为了在应用中能匹配路径，在创建`<Route>`元素时必须带有需要匹配的`path`作为参数

```javascript
<Switch>
  <Route exact path='/' component={Home}/>
  {/* both /roster and /roster/:number begin with /roster */}
  <Route path='/roster' component={Roster}/>
  <Route path='/schedule' component={Schedule}/>
</Switch>
```

**<Route>是如何渲染的？**

> 当一个路由的`path`匹配成功后，路由用来确定渲染结果的参数有三种。只需要提供其中一个即可

- `component` ： 一个`React`组件。当带有`component`参数的`route`匹配成功后，`route`会返回一个新的元素，其为`component`参数所对应的`React`组件（使用`React.createElement`创建）。
- `render` ： 一个返回`React element`的函数。当匹配成功后调用该函数。该过程与传入`component`参数类似，并且对于行级渲染与需要向元素传入额外参数的操作会更有用。
- `children` ： 一个返回`React element`的函数。与上述两个参数不同，无论`route`是否匹配当前`location`，其都会被渲染

```javascript
<Route path='/page' component={Page} />
const extraProps = { color: 'red' }
<Route path='/page' render={(props) => (
  <Page {...props} data={extraProps}/>
)}/>
<Route path='/page' children={(props) => (
  props.match
    ? <Page {...props}/>
    : <EmptyPage {...props}/>
)}/>
```

> 通常`component`参数与`render`参数被更经常地使用。`children`参数偶尔会被使用，它更常用在`path`无法匹配时呈现的'空'状态。在本例中并不会有额外的状态，所以我们将使用`<Route>`的`component`参数

- 通过`<Route>`渲染的元素会被传入一些参数。分别是`match`对象，当前`location`对象以及`history`对象（由`router`创建）

**<Main>**

> 现在我们清楚了根路由的结构，我们需要实际渲染我们的路由。对于这个应用，我们将会在`<Main>`组件中渲染`<Switch>`与`<Route>`，这一过程会将`route`匹配生成的`HTML`放在`<main>`节点中

```javascript
import { Switch, Route } from 'react-router-dom'
const Main = () => (
  <main>
    <Switch>
      <Route exact path='/' component={Home}/>
      <Route path='/roster' component={Roster}/>
      <Route path='/schedule' component={Schedule}/>
    </Switch>
  </main>
)
```

- **注意**：主页路由包含额外参数。该参数用来保证路由能准确匹配`path`

**嵌套路由**

> 路由`/roster/:number`并未包含在上述`<Switch>`中。它由`<Roster>`组件负责在路径包含`'/roster'`的情形下进行渲染

- 在`<Roster>`组件中，我们将为两种路径进行渲染
  - `/roster`：对应路径名仅仅是`/roster`时，因此需要在`exact`元素上添加`exact`参数
  - `/roster/:number`该路由使用一个路由参数来获取/roster后的路径名
  
```javascript
const Roster = () => (
<Switch>
    <Route exact path='/roster' component={FullRoster}/>
    <Route path='/roster/:number' component={Player}/>
</Switch>
)
```
> 组合在相同组件中分享共同前缀的路由是一种有用的方法。这就需要简化父路由并且提供一个区域来渲染具有相同前缀的通用路由

例如，`<Roster>`用来渲染所有以`/roster`开始的全部路由

```javascript
const Roster = () => (
  <div>
    <h2>This is a roster page!</h2>
    <Switch>
      <Route exact path='/roster' component={FullRoster}/>
      <Route path='/roster/:number' component={Player}/>
    </Switch>
  </div>
)
```

**路径参数**

有时路径名中存在我们需要获取的参数。例如，在运动员界面，我们需要获取运动员的编号。我们可以向`route`的路径字符串中添加`path`参数

- 如`'/roster/:number'`中`:number`这种写法意味着`/roster/`后的路径名将会被获取并存在`match.params.number`中。例如，路径名`'/roster/6'`会获取到一个对象

```javascript
{ number: '6' } // 注获取的值是字符串类型的
```

- `<Player>`组件可以使用`props.match.params`对象来确定需要被渲染的运动员的数据

```javascript
// 返回运动员对象的API
import PlayerAPI from './PlayerAPI'
const Player = (props) => {
  const player = PlayerAPI.get(
    parseInt(props.match.params.number, 10)
  )
  if (!player) {
    return <div>Sorry, but the player was not found</div>
  }
  return (
    <div>
      <h1>{player.name} (#{player.number})</h1>
      <h2>{player.position}</h2>
    </div>
)
```

除了`<Player>`组件，我们的页面还包含`<FullRoster>`, `<Schedule>`以及 `<Home>`组件

```javascript
const FullRoster = () => (
  <div>
    <ul>
      {
        PlayerAPI.all().map(p => (
          <li key={p.number}>
            <Link to={`/roster/${p.number}`}>{p.name}</Link>
          </li>
        ))
      }
    </ul>
  </div>
)
const Schedule = () => (
  <div>
    <ul>
      <li>6/5 @ Evergreens</li>
      <li>6/8 vs Kickers</li>
      <li>6/14 @ United</li>
    </ul>
  </div>
)
const Home = () => (
  <div>
    <h1>Welcome to the Tornadoes Website!</h1>
  </div>
)
```

七、Link
---

> 现在，我们应用需要在各个页面间切换。如果使用锚点元素（就是）实现，在每次点击时页面将被重新加载。`React Router`提供了`<Link>`组件用来避免这种状况的发生。当你点击`<Link>`时，`URL`会更新，组件会被重新渲染，但是页面不会重新加载

```javascript
import { Link } from 'react-router-dom'
const Header = () => (
  <header>
    <nav>
      <ul>
        <li><Link to='/'>Home</Link></li>
        <li><Link to='/roster'>Roster</Link></li>
        <li><Link to='/schedule'>Schedule</Link></li>
      </ul>
    </nav>
  </header>
)
```

- `<Link>`使用`'to'`参数来描述需要定位的页面。它的值即可是字符串也可是`location`对象（包含`pathname`，`search`，`hash`与`state`属性）。如果其值为字符床将会被转换为`location`对象。

```html
<Link to={{ pathname: '/roster/7' }}>Player #7</Link>
```

八、注释
----

- `locations` 是一个含有描述U`RL`不同部分属性的对象

```javascript
// 一个基本的location对象
{ pathname: '/', search: '', hash: '', key: 'abc123' state: {} }
```

- 如果你使用`children`参数，即便在当前`location`不匹配时`route`也将进行渲染
