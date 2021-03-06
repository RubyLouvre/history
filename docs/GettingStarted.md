## Getting Started

The first thing you'll need to do is create a [history object](Glossary.md#history). The main `history` module exports several different [`create*` methods](Glossary.md#createhistory) that you can use depending on your environment.

- `createHistory` is for use in modern web browsers that support the [HTML5 history API](http://diveintohtml5.info/history.html) (see [cross-browser compatibility](http://caniuse.com/#feat=history))
- `createHashHistory` is for use in legacy web browsers (see [caveats of using hash history](HashHistoryCaveats.md))
- `createMemoryHistory` is used mainly for testing and as a reference implementation

Once you get a `history` object, use `history.listen` to be notified when [the `location`](Location.md) changes.

```js
import { createHistory } from 'history'

const history = createHistory()

// Get the current location.
const location = history.getCurrentLocation()

// Listen for changes to the current location.
const unlisten = history.listen(location => {
  console.log(location.pathname)
})

// When you're finished, stop the listener.
unlisten()
```

### Navigation

You can also use a `history` object to programmatically change the current `location` using the following methods:

- `push(location)`
- `replace(location)`
- `go(n)`
- `goBack()`
- `goForward()`

The `push` and `replace` methods accept a single `location` argument as either:

1. A [path string](Glossary.md#path) that represents a complete URL path, including the [search string](Glossary.md#search) and [hash](Glossary.md#hash) or
2. A [location descriptor](Glossary.md#locationdescriptor) object that uses a combination of [`pathname`](Glossary.md#pathname), [`search`](Glossary.md#search), [`hash`](Glossary.md#hash), and [`state`](Glossary.md#locationstate) properties

```js
// Push a new entry onto the history stack.
history.push('/home')

// Replace the current entry on the history stack.
history.replace('/profile')

// Push a new entry with state onto the history stack.
// state must be a JSON-serializable object (no Function
// or Date values).
history.push({
  pathname: '/about',
  search: '?the=search',
  state: { some: 'state' }
})

// Push a new entry onto the history stack that changes
// only the search on an existing location.
history.push({ ...location, search: '?the=other+search' })

// Go back to the previous history entry. The following
// two lines are synonymous.
history.go(-1)
history.goBack()
```

To prevent the user from navigating away from a page, or to prompt them before they do, see the documentation on [confirming navigation](ConfirmingNavigation.md).

### Creating URLs

Additionally, `history` objects can be used to create URL paths and/or `href`s for `<a>` tags that link to various places in your app. This is useful when using hash history to prefix URLs with a `#` or when using [query support](QuerySupport.md) to automatically build query strings.

```js
const href = history.createHref('/the/path')
```

### Minimizing Your Build

Using the main `history` module is a great way to get up and running quickly. However, you probably don't need to include all the various history implementations in your production bundle. To keep your build as small as possible, import only the functions you need directly from `history/lib`.

```js
// HTML5 history, recommended
import createHistory from 'history/lib/createBrowserHistory'

// Hash history
import createHistory from 'history/lib/createHashHistory'

// Memory history
import createHistory from 'history/lib/createMemoryHistory'
```
