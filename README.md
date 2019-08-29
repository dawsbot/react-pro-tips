![](https://miro.medium.com/max/1400/1*nr79p-m8ki2L3zepg1nc8g.gif)
<p align="center">
  <b>🙌 Take your React skills beyond average 🙌</b>
</p>

<br />

# Performance

## Avoid inline functions in render

```jsx
// Replace this
render() {
  return (
    <MyComponent onClick={value => this.setState({ value })} />
  );
}

// With this
handleClick = value => this.setState({ value });

render() {
  return (
    <MyComponent onClick={this.handleClick} />
  )
}
```

#### Why?

Having a function inline **creates a diff in props every time the parent is rendered**. If this is high in your component tree, you can create a slow application with UI jank.

[Read more here](https://maarten.mulders.it/2017/07/no-bind-or-arrow-functions-in-in-jsx-props-why-how/)

<br />

## Define constants outside of jsx

> This includes objects, arrays, React components, and (as we've already mentioned above) functions

```jsx
// replace this
const RedDiv = () => <div style={{color: 'red'}}/>
```

```jsx
// with this
const redStyle = {color: 'red'}
const RedDiv = () => <div style={redStyle}/>
```

#### Why?

Creating a new array, object, or functions within jsx creates a diff when there is none. If this is high in your component tree, you can create a slow application with UI jank.

<br />

## Access props and state from `this` whenever possible

When a class calls a helper function that's also in that class, do not pass `props` nor `state` into that helper function. We should desctructure from `this` at the top of the helper function instead.

```jsx
// Replace this
handleClick = (isError) => {
  isError ? doThing('red') : doThing('green')
}

render() {
  const {isError} = this.props;
  return (
    <div onClick={() => this.handleClick(isError)>
        Click me
    </div>
  );
}
```

```jsx
// With this
handleClick = () => {
  this.props.isError ? doThing('red') : doThing('green')
}

render() {
  return (
    <div onClick={this.handleClick}>
        Click me
    </div>
  );
}
```

#### Why?

Providing unecessary props creates performance issues. In the example above, we were able to avoid an inline function because of the refactor 👏

In addition, there is type safety lost when a function receives a prop. This can be fixed if you manually type the function which receives this prop, but that's unecessary code bloat, which rots over time (tested in TypeScript).

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

In the beginning, I find it easiest to do this for only the biggest routes. Find the biggest routes with [webpack-bundle-analyzer](https://github.com/webpack-contrib/webpack-bundle-analyzer) or [source-map-explorer](https://github.com/danvk/source-map-explorer)

#### Why?

Page load speeds are directly correlated with revenue, retention, etc.

[Read more here](https://css-tricks.com/using-react-loadable-for-code-splitting-by-components-and-routes/)

<br />

---

# Follow-ups

If you would like consulting, contact us at [DarkTriangle.tech](https://darktriangle.tech)

<p align="center">
  <img src="https://avatars2.githubusercontent.com/u/49670561" width="400"/>
</p>
