# Crayon
## React middleware

### Getting started

```jsx
import React from 'react'
import ReactDOM from 'react-dom'
import * as crayon from 'crayonjs'
import * as react from 'crayonjs/react'

const app = crayon.create()

app.use(react.Router('#app', React, ReactDOM))

app.path('/', (req, res) => res.mount(() => <div>Hello World</div>))

app.load()
```

### Lazy Loading

Lazy loading simply requires you to use the latest JavaScript dynamic module feature `import()`

```javascript
app.path('/', async (req, res) => {
    const HomeView = await import('./home-view')
    res.mount(HomeView)
})
```

### Route Groups

Groups (at least at this stage) are simply functions that take a `router` instance and register their routes.

```javascript
import { UsersView, UsersDetailView } from './views'

const usersGroup = (app) => {
    app.path('/users', (req, res) =>
        res.mount(UsersView)
    )

    app.path('/users/:id', (req, res) =>
        res.mount(UsersDetailView)
    )
}
```

```javascript
import * as crayon from 'crayon';
import * as react from 'crayon/react';
import { usersGroup } from './users'

const app = crayon.create()

app.use(react.Router('#app', React, ReactDOM))
usersGroup(app)

app.path('/', (req, res) =>
    res.mount(views.HomeView)
)

app.load()
```

### Dealing With Dependencies

I recomend using a parameter injection model for dependency injection

```jsx
export const MyView = (dep) => () => <div>{ dep.value }<div>
```

```javascript
import * as crayon from 'crayon';
import * as react from 'crayon/react';
import { MyView } from './views'

const dep = { value: 'hello world' }
const app = crayon.create()

app.use(react.Router('#app', React, ReactDOM))

app.path('/', (req, res) => res.mount(MyView(dep)))

app.load()
```