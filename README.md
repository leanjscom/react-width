# React Width

## Why do we need this Width?

React is a paradigm shift, and it changes we way we think of time from what we are used to when implementing UI using other tools. For instance, we should not hide things based on events, but instead just not display them.

material-ui.com (MUI) has an interesting utility to help us achieving that, [withWidth](https://github.com/callemall/material-ui/blob/master/src/utils/withWidth.js). withWidth is a Higher-Order Component (HOC) that tells a component what type of screen is display the component. It uses props (like any other HOC) to inject some properties into a component. The injected prop in this case is `width`, which is the width of the screen. The width of the screen is represented in these 3 values depending on the size:

```javascript
const LARGE = 3
const MEDIUM = 2
const SMALL = 1
```

This way a component that is enhanced with withWidth can check props.width to make decissions about about how the interface looks like. For instance, in a page with 2 columns, if the screen is `LARGE` then we display the left column, if the screen is `MEDIUM` or `SMALL`, the we display the right column.

```javascript
  <Row>
    { pros.width < LARGE ? <Right /> : <Left /> }
  </Row>
```

You could achieve the same using CSS media queries, but the point is, you don't have to hide something that is not on the screen.


## Why have we created another util that does the same as MUI withWidth?

1- It's not coupled to MUI. We think this util is great, and you should apply this idea in any React project. So if you don't use MUI you can just install this package.

2- We think [Bootstrap](http://getbootstrap.com/) is a well designed CSS framework, and there has been a lot of thinking on it. There are many decision that we have made that we find useful and we use. One of them is the separation of screen sizes is more granular than MUI. Bootstrap defines these sizes: extra small, small, medium, large, and extra large. So this package uses the same number of sizes of Bootstrap and keeps compatibility with MUI value:

```javascript
const EXTRA_LARGE = 4
const LARGE = 3
const MEDIUM = 2
const SMALL = 1
const EXTRA_SMALL = 0
```

3- MUI implements the `width` functionality using a HOC. We think HOC are great, but there are some cases where [Render Callbacks](http://reactpatterns.com/#render-callback) (AKA [Function as Child](https://medium.com/merrickchristensen/function-as-child-components-5f3920a9ace9) works better. Therefore we have implemented both so you can choose which one suits your problem. To be more precise, we have just implemented a Width Render Callback Component, and our withWidth just uses the Width Render Callback Component.

## How use it

### Render Callback flavour

```react

import { Width, LARGE } from 'react-width'

const NewFloatingButton = ({ onClick }) => (
  <Width>
    {(width) => {
      const styles = { position: 'fixed', top: '15px', right: '15px' }
      let mini = true
      if (width === LARGE) {
        mini = false
      }

      return (
        <FloatingActionButton onClick={onClick} secondary mini={mini} style={styles}>
          <ContentAdd />
        </FloatingActionButton>
      )
    }}
  </Width>
)

export default NewFloatingButton

```

### HOC flavour

```react

import withWidth, { LARGE } from 'react-width'

const NewFloatingButton = ({ onClick, width }) => {
  const styles = { position: 'fixed', top: '15px', right: '15px' }
  let mini = true
  if (width === LARGE) {
    mini = false
  }

  return (
    <FloatingActionButton onClick={onClick} secondary mini={mini} style={styles}>
      <ContentAdd />
    </FloatingActionButton>
  )
}

export default withWidth(NewFloatingButton)

```
