<a href="#about"><img alt="logo" width="1016" alt="2017-07-02 1 46 12" src="https://user-images.githubusercontent.com/8784712/27764289-6ffb8ab8-5ec8-11e7-8b30-9b59ecc1d47d.png"></a>

## About

This is not yet another minimalistic React implementation, the primary use case is to create actual dom with a single function and JSX directly, but there're opt-in component lifecycle hooks.

This lib does not accept pull requests for new features since the primary use case (get actual DOM from JSX) is already set for good.

## Features

- [x] Insanely small: 2kB minified
- [x] One API: JSX is transformed to vNode, use `mount(vNode)` to get actual DOM.
- [x] SVG support
- [x] Protection from XSS injections
- [x] Automatically joining classNames, styles

## Install

```bash
yarn add dom-dom
```

CDN: [UNPKG](https://unpkg.com/dom-dom/dist/) | [jsDelivr](https://cdn.jsdelivr.net/npm/dom-dom/dist/)

## Usage

With a transpiler like `babel+babel-plugin-transform-react-jsx` or `typescript` or `buble`:

```js
/* @jsx h */
import { h, mount } from 'dom-dom/tiny'

// With only first arg
const button = mount(<button>click me</button>)
// button.outerHTML:
// => '<button>click me</button>'

// With second arg
// replacce `#root` with created element
mount(
  <button>hello</button>, 
  document.getElementById('root')
)
```

Note that while using CDN version you can access `d2.h` `d2.mount` instead.

### className

`className` can be `string` `Array` or `Object`:

```js
<div className="foo"></div>
<div className={['foo', 'bar']}></div>
<div className={{foo: false, [`bar-${index}`]: true}}></div>
```

You can also directly use `class` instead of react-specific `className` as you please:

```js
<div class="foo"></div>
```

### style

`style` supports `string` and `Object`:

```js
<div style="color: red"></div>
// both kebab-case and camelCase are supported here
// default unit is `px`
<div style={{ fontSize: 14, 'background-color': 'red' }}></div>
```

### innerHTML

```js
<div dangerouslySetInnerHTML={{__html: '<strong>hey</strong>'}}></div>
```

### Events

React-like events are supports:

```js
<button onClick={handleClick}></button>
```

---

> **WARNING:** If you only want a function to transform vNode to actual dom, please stop reading!!! Above features would be enough for your use case. Following features may not be what you want :D
> To use full build you should `import { xxx } from 'dom-dom'` instead.

<details><summary>Create your own React with <strong>dom-dom</strong></summary><br>

```js
// @jsx h

import { h, mount, unmount } from 'dom-dom'

class Component {
  setState(state) {
    if (typeof state === 'function') {
      state = state(this.state)
    }
    for (const key in state) {
      this.state[key] = state[key]
    }
    this.mount()
  }

  mount(root = this.$root) {
    this.$root = mount(this, root)
    return this.$root
  }
  
  destroy = () => {
    unmount(this, this.$root)
  }

}

class Counter extends Component {
  state = { count: 0 }

  handleClick = () => {
    this.setState(prevState => ({
      count: prevState.count + 1
    }))
  }
  
  componentDidMount() {
    console.log('app mounted!', this)
  }
  
  render() {
    return (<div>
      <button onClick={this.handleClick}>
        clicked: {this.state.count} times
      </button>
      <button onClick={this.destroy}>destroy</button>
    </div>)
  }
}

const counter = new Counter()
counter.mount(document.getElementById('root'))
```

[![Edit 9Q4n4XxAP](https://codesandbox.io/static/img/play-codesandbox.svg)](https://codesandbox.io/s/VyGn0DP5)
</details><br>

You can `mount` `unmount` a object or class instance which has a `render` method that returns `vNode`.

```js
import { h, mount } from 'dom-dom'

const A = {
  render() {
    return <div>a</div>
  }
}

const B = class {
  render() {
    return <div>{A}</div>
  }
}

mount(new B, document.getElementById('root'))
```

Object and class instance with render function can also be one of your JSX children.

This is designed for using lifecycle hooks, currently we have `componentDidMount` `componentWillMount` and `componentWillUnmount`.

```js
const App = {
  componentDidMount() {
    console.log('hi')
  },
  componentWillUnmount() {
    console.log('bye')
  },
  render() {
    return <div>hi</div>
  }
}

const root = mount(App, document.getElementById('root'))
//=> hi
unmount(App, root)
//=> bye
```

## Prior Art

This project is heavily inspired by [preact](https://github.com/developit/preact) and [dom-chef](https://github.com/vadimdemedes/dom-chef).

## Contributing

1. Fork it!
2. Create your feature branch: `git checkout -b my-new-feature`
3. Commit your changes: `git commit -am 'Add some feature'`
4. Push to the branch: `git push origin my-new-feature`
5. Submit a pull request :D

## Badges

[![NPM version](https://img.shields.io/npm/v/dom-dom.svg?style=flat)](https://npmjs.com/package/dom-dom) [![NPM downloads](https://img.shields.io/npm/dm/dom-dom.svg?style=flat)](https://npmjs.com/package/dom-dom) [![CircleCI](https://circleci.com/gh/egoist/dom-dom/tree/master.svg?style=shield&circle-token=1b6201de2b133f5b995fe2730a24b497768d85c6)](https://circleci.com/gh/egoist/dom-dom/tree/master)  [![donate](https://img.shields.io/badge/$-donate-ff69b4.svg?maxAge=2592000&style=flat)](https://github.com/egoist/donate)

## Author

**dom-dom** © [egoist](https://github.com/egoist), Released under the [MIT](./LICENSE) License.<br>
Authored and maintained by egoist with help from contributors ([list](https://github.com/egoist/dom-dom/contributors)).

> [egoistian.com](https://egoistian.com) · GitHub [@egoist](https://github.com/egoist) · Twitter [@rem_rin_rin](https://twitter.com/rem_rin_rin)
