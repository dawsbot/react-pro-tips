![](https://miro.medium.com/max/1400/1*nr79p-m8ki2L3zepg1nc8g.gif)
<p align="center">
  <b>🙌 Take your React skills beyond average 🙌</b>
</p>

<br />

# Performance

## Avoid inline functions in props

```jsx
// Replace this
<MyComponent onClick={value => this.setState({ value })}

// With this
const handleClick = value => this.setState({ value });
<MyComponent onClick={this.handleClick} />
```

#### Why?

Having a function inline **creates a diff in props every time the parent is rendered**. If this is high in your component tree, you can create a slow application with UI jank.

[Read more here](https://maarten.mulders.it/2017/07/no-bind-or-arrow-functions-in-in-jsx-props-why-how/)

<br />

## Split-up your bundle

If you're using create-react-app or a similar tool, your **entire JavaScript bundle is delivered on the first page load**. 

You can cut this in half or more by adding in a package like [https://github.com/jamiebuilds/react-loadable](https://github.com/jamiebuilds/react-loadable)

```jsx
// Replace this
import HomePage from './pages/HomePage';

// With this
import loadable from 'react-loadable';
const HomePage = loadable({
  loader: () =>
    import(
      /* webpackChunkName: "route-home-page" */ './pages/HomePage'
    ),
  loading,
});
```

In the beginning, I find it easiest to do this for only the biggest routes. Find the biggest routes with [webpack-bundle-analyzer](https://github.com/webpack-contrib/webpack-bundle-analyzer) or  [source-map-explorer](https://github.com/danvk/source-map-explore)

#### Why?

Page load speeds are directly correlated with revenue, retention, etc.

[Read more here](https://css-tricks.com/using-react-loadable-for-code-splitting-by-components-and-routes/)

<br />

---

# Follow-ups

If you would like consulting, you can learn more about my offerings at [DarkTriangle.tech](https://darktriangle.tech)

<p align="center">
  <img src="https://avatars2.githubusercontent.com/u/49670561" width="400"/>
</p>
