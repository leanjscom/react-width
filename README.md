# React Width

## How to install:
```sh
yarn add react-width
```

## Why do we need this Width?

React is a paradigm shift, and it changes the way we think of "time" from what we are used to when implementing UIs using other tools. For instance, we should not hide things based on events, but instead just not display them.

material-ui.com (MUI) has an interesting utility to help us achieve this, [withWidth](https://github.com/callemall/material-ui/blob/master/src/utils/withWidth.js). withWidth is a Higher-Order Component (HOC) that tells a component what size of screen is currently displaying the component. It uses props (like any other HOC) to inject some properties into a component. The injected prop in this case is `width`, which is the width of the screen. The width of the screen is represented in these 3 values:

```javascript
const LARGE = 3
const MEDIUM = 2
const SMALL = 1
```

This way a component that is enhanced with withWidth can check props.width to make decissions about how the interface should be. For instance, in a page with 2 columns, if the screen is `LARGE` then we display the left column, if the screen is `MEDIUM` or `SMALL`, then we display the right column:

```javascript
  <Row>
    { pros.width < LARGE ? <Right /> : <Left /> }
  </Row>
```

You could achieve the same using CSS media queries, but the point is you don't have to hide something that is not on the screen.


## Why have we created another util that does the same as MUI withWidth does?

1- It's not coupled to MUI. We think this util is great, and you should apply this idea in any React project. So if you don't use MUI you can just install this package.

2- We think [Bootstrap](http://getbootstrap.com/) is a well designed CSS framework, and there has been a lot of thought put into it. Bootstrap has made many decisions that we find correct, and so we utilize them. One of these, is that the separation of screen sizes is more granular than it is in MUI. Bootstrap defines these screen sizes: extra small, small, medium, large, and extra large. Therefore this package uses the same number of sizes as Bootstrap, while maintaining compatibility with MUI values:

```javascript
const EXTRA_LARGE = 4
const LARGE = 3
const MEDIUM = 2
const SMALL = 1
const EXTRA_SMALL = 0
```

3- MUI implements the `width` functionality using a HOC. We think HOCs are great, but there are some cases where [Render Callback](http://reactpatterns.com/#render-callback) (AKA) [Function as Child](https://medium.com/merrickchristensen/function-as-child-components-5f3920a9ace9) works better. Therefore we have implemented both so you can choose the one that best suits your problem. To be more precise, we have just implemented a Width Render Callback Component, and our withWidth just uses the Width Render Callback Component.

## How to use it

### Render Callback implementation

```javascript

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

### HOC implementation

```javascript

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
