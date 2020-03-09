##### React-router使用
- 路由模式（hash、H5 history）,同vue-router
  - hash模式（默认），如http://abc.com/#/user/10
  ```
  import React from 'react'
  import {
    HashRouter as Router,
    Switch,
    Route
  } from 'react-router-dom'
  ```
  - H5 history模式，如http://abc.com/user/20
  ```
  import React from 'react'
  import {
    BrowserRouter as Router,
    Switch,
    Route
  } from 'react-router-dom'
  ```
  - 后者需要server端支持，因此无特殊需求可选择前者
  ```
  function RouterComponent() {
    return (
      <Router>
        <Switch>
          <Route exact path="/">
            <Home />
          </Route>
          <Route exact path="/project/:id">
            <Project />
          </Route>
          <Route exact path="*">
            <NotFound />
          </Route>
        </Switch>
      </Router>
    )
  }
  ```
- 路由配置（动态路由、懒加载）,同vue-router
```
import {BrowserRouter as Router, Route, Swtch} from 'react-router-dom'
import React, { Suspense, lazy } from 'react'

const Home = lazy(() => import('./routes/Home'))
const About = lazy(() => import('./routes/About'))

const App = () => (
  <Router>
    <Suspense fallback={<div>Loading...</div>}>
      <Switch>
        <Route exact path="/" component={Home}/>
        <Route path="/about" component={About}/>
      </Switch>
    </Suspense>
  </Router>
)
```
