# React/JSX Style Guide

*A mostly reasonable approach to React and JSX*

## Table of Contents

  1. [Basic Rules](#basic-rules)
  1. [Class vs `React.createClass` vs stateless](#class-vs-reactcreateclass-vs-stateless)
  1. [Naming](#naming)
  1. [Declaration](#declaration)
  1. [Alignment](#alignment)
  1. [Quotes](#quotes)
  1. [Spacing](#spacing)
  1. [Props](#props)
  1. [Refs](#refs)
  1. [Parentheses](#parentheses)
  1. [Tags](#tags)
  1. [Methods](#methods)
  1. [Ordering](#ordering)
  1. [`isMounted`](#ismounted)
  1. [Reference](#reference)

## Basic Rules

  - Only include one React component per file.
    - However, multiple [Stateless, or Pure, Components](https://facebook.github.io/react/docs/reusable-components.html#stateless-functions) are allowed per file. eslint: [`react/no-multi-comp`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-multi-comp.md#ignorestateless).
  - Always use JSX syntax.
  - Do not use `React.createElement` unless you're initializing the app from a file that is not JSX.

## Class vs `React.createClass` vs stateless

  - If you have internal state and/or refs, use `class extends React.Component`. Facebook is phasing out `React.createClass` and they have recently decided that mixins constitute an antipattern.

    ```jsx
    // bad
    const Listing = React.createClass({
      // ...
      render () {
        return <div>{this.state.hello}</div>
      }
    })

    // good
    class Listing extends React.Component {
      // ...
      render () {
        return <div>{this.state.hello}</div>
      }
    }
    ```

    And if you don't have state or refs, prefer arrow functions over classes:

    ```jsx
    // bad
    class Listing extends React.Component {
      render () {
        return <div>{this.props.hello}</div>
      }
    }

    // good
    const Listing = ({ hello }) => (
      <div>{hello}</div>
    )
    ```

## Naming

  - **Extensions**: Use `.js` extension for React components.
  - **Filename**: Use SnakeCase for filenames. E.g., `reservation_card.js`.
  - **Reference Naming**: Use PascalCase for React components and camelCase for their instances. eslint: [`react/jsx-pascal-case`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-pascal-case.md)

    ```jsx
    // bad
    import reservationCard from './reservation_card'

    // good
    import ReservationCard from './reservation_card'

    // bad
    const ReservationItem = <ReservationCard />

    // good
    const reservationItem = <ReservationCard />
    ```

  - **Component Naming**: Use the filename as the component name. For example, `reservation_card.js` should have a reference name of `ReservationCard`. However, for root components of a directory, use `index.js` as the filename and use the directory name as the component name:

    ```jsx
    // bad
    import Footer from './footer/footer'

    // bad
    import Footer from './footer/index'

    // good
    import Footer from './footer'
    ```

## Declaration

  - Do not use `displayName` for naming components. Instead, name the component by reference.

    ```jsx
    // bad
    export default React.createClass({
      displayName: 'ReservationCard',
      // stuff goes here
    });

    // good
    export default class ReservationCard extends React.Component {
    }
    ```

## Alignment

  - Follow these alignment styles for JSX syntax. eslint: [`react/jsx-closing-bracket-location`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-closing-bracket-location.md)

    ```jsx
    // bad
    <Foo superLongParam='bar'
         anotherSuperLongParam='baz' />

    // good
    <Foo
      superLongParam='bar'
      anotherSuperLongParam='baz' />

    // if props fit in one line then keep it on the same line
    <Foo bar='bar' />

    // children get indented normally
    <Foo
      superLongParam='bar'
      anotherSuperLongParam='baz'>
      <Quux />
    </Foo>
    ```

## Quotes

  - Always use single quotes (`'`) for JSX attributes. eslint: [`jsx-quotes`](http://eslint.org/docs/rules/jsx-quotes)

    > Why? Following the same [JavaScript Standard Style](http://standardjs.com) as any of our other JavaScript code.

    ```jsx
    // bad
    <Foo bar="bar" />

    // good
    <Foo bar='bar' />

    // bad
    <Foo style={{ left: "20px" }} />

    // good
    <Foo style={{ left: '20px' }} />
    ```

## Spacing

  - Always include a single space in your self-closing tag.

    ```jsx
    // bad
    <Foo/>

    // very bad
    <Foo                 />

    // bad
    <Foo
     />

    // good
    <Foo />
    ```

  - Do not pad JSX curly braces with spaces. eslint: [`react/jsx-curly-spacing`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-curly-spacing.md)

    ```jsx
    // bad
    <Foo bar={ baz } />

    // good
    <Foo bar={baz} />
    ```

## Props

  - Always use camelCase for prop names.

    ```jsx
    // bad
    <Foo
      UserName='hello'
      phone_number={12345678} />

    // good
    <Foo
      userName='hello'
      phoneNumber={12345678} />
    ```

  - Omit the value of the prop when it is explicitly `true`. eslint: [`react/jsx-boolean-value`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-boolean-value.md)

    ```jsx
    // bad
    <Foo hidden={true} />

    // good
    <Foo hidden />
    ```

  - Always include an `alt` prop on `<img>` tags. If the image is presentational, `alt` can be an empty string or the `<img>` must have `role="presentation"`. eslint: [`jsx-a11y/img-has-alt`](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/img-has-alt.md)

    ```jsx
    // bad
    <img src='hello.jpg' />

    // good
    <img src='hello.jpg' alt='Me waving hello' />

    // good
    <img src='hello.jpg' alt='' />

    // good
    <img src='hello.jpg' role='presentation' />
    ```

  - Do not use words like "image", "photo", or "picture" in `<img>` `alt` props. eslint: [`jsx-a11y/img-redundant-alt`](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/img-redundant-alt.md)

    > Why? Screenreaders already announce `img` elements as images, so there is no need to include this information in the alt text.

    ```jsx
    // bad
    <img src='hello.jpg' alt='Picture of me waving hello' />

    // good
    <img src='hello.jpg' alt='Me waving hello' />
    ```

  - Use only valid, non-abstract [ARIA roles](https://www.w3.org/TR/wai-aria/roles#role_definitions). eslint: [`jsx-a11y/aria-role`](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/aria-role.md)

    ```jsx
    // bad - not an ARIA role
    <div role='datepicker' />

    // bad - abstract ARIA role
    <div role='range' />

    // good
    <div role='button' />
    ```

  - Do not use `accessKey` on elements. eslint: [`jsx-a11y/no-access-key`](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/no-access-key.md)

    > Why? Inconsistencies between keyboard shortcuts and keyboard commands used by people using screenreaders and keyboards complicate accessibility.

    ```jsx
    // bad
    <div accessKey='h' />

    // good
    <div />
    ```

  - Avoid using an array index as `key` prop, prefer a unique ID. ([why?](https://medium.com/@robinpokorny/index-as-a-key-is-an-anti-pattern-e0349aece318))

    ```jsx
    // bad
    {todos.map((todo, index) => {
        return (
        <Todo
            {...todo}
            key={index} />
        )
    })}

    // good
    {todos.map(todo => {
        return (
        <Todo
            {...todo}
            key={todo.id} />
        )
    })}
    ```

## Refs

  - Always use ref callbacks. eslint: [`react/no-string-refs`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-string-refs.md)

    ```jsx
    // bad
    <Foo ref='myRef' />

    // good
    <Foo
      ref={ref => this.myRef = ref} />
    ```

## Parentheses

  - Wrap JSX tags in parentheses when they span more than one line. eslint: [`react/wrap-multilines`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/wrap-multilines.md)

    ```jsx
    // bad
    render () {
      return <MyComponent className='long body' foo='bar'>
               <MyChild />
             </MyComponent>
    }

    // good
    render () {
      return (
        <MyComponent className='long body' foo='bar'>
          <MyChild />
        </MyComponent>
      )
    }

    // good, when single line
    render () {
      const body = <div>hello</div>

      return <MyComponent>{body}</MyComponent>
    }
    ```

## Tags

  - Always self-close tags that have no children. eslint: [`react/self-closing-comp`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/self-closing-comp.md)

    ```jsx
    // bad
    <Foo className='stuff'></Foo>

    // good
    <Foo className='stuff' />
    ```

  - If your component has multi-line properties, close its tag on the last property. eslint: [`react/jsx-closing-bracket-location`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-closing-bracket-location.md)

    ```jsx
    // bad
    <Foo
      bar='bar'
      baz='baz'
    />

    // good
    <Foo
      bar='bar'
      baz='baz' />
    ```

## Methods

  - Use arrow functions to close over local variables.

    ```jsx
    function ItemList (props) {
      return (
        <ul>
          {props.items.map((item, index) => {
            <Item
              key={item.key}
              onClick={() => doSomethingWith(item.name, index)} />
          })}
        </ul>
      );
    }
    ```

  - Bind event handlers for the render method in the constructor. eslint: [`react/jsx-no-bind`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-no-bind.md)

    > Why? A bind call in the render path creates a brand new function on every single render.

    ```jsx
    // bad
    class extends React.Component {
      onClickDiv () {
        // do stuff
      }

      render () {
        return <div onClick={this.onClickDiv.bind(this)} />
      }
    }

    // good
    class extends React.Component {
      constructor (props, context) {
        super(props, context)

        this.onClickDiv = this.onClickDiv.bind(this)
      }

      onClickDiv () {
        // do stuff
      }

      render () {
        return <div onClick={this.onClickDiv} />
      }
    }
    ```

  - Do not use underscore prefix for internal methods of a React component.

    ```jsx
    // bad
    React.createClass({
      _onClickSubmit () {
        // do stuff
      },

      // other stuff
    });

    // good
    class extends React.Component {
      onClickSubmit () {
        // do stuff
      }

      // other stuff
    }
    ```

  - Be sure to return a value in your `render` methods. eslint: [`require-render-return`](https://github.com/yannickcr/eslint-plugin-react/pull/502)

    ```jsx
    // bad
    render () {
      (<div />)
    }

    // good
    render () {
      return (<div />)
    }
    ```

## Ordering

  - Ordering for `class extends React.Component`:

  1. `constructor`
  1. optional `static` methods
  1. `getChildContext`
  1. `componentWillMount`
  1. `componentDidMount`
  1. `componentWillReceiveProps`
  1. `shouldComponentUpdate`
  1. `componentWillUpdate`
  1. `componentDidUpdate`
  1. `componentWillUnmount`
  1. `render`
  1. *clickHandlers or eventHandlers* like `onClickSubmit()` or `onChangeDescription()`
  1. *getter methods for `render`* like `getSelectReason()` or `getFooterContent()`
  1. *Optional render methods* like `renderNavigation()` or `renderProfilePicture()`

  - How to define `propTypes`, `defaultProps`, `contextTypes`, etc...

    ```jsx
    import React, { PropTypes } from 'react';

    export default class Link extends React.Component {
      static propTypes = {
        id: PropTypes.number.isRequired,
        url: PropTypes.string.isRequired,
        text: PropTypes.string
      }

      static defaultProps = {
        text: 'Hello World',
      }

      render () {
        const { id, url, text } = this.props

        return <a href={url} data-id={id}>{text}</a>
      }
    }
    ```

## `isMounted`

  - Do not use `isMounted`. eslint: [`react/no-is-mounted`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-is-mounted.md)

    > Why? [`isMounted` is an anti-pattern][anti-pattern], is not available when using ES6 classes, and is on its way to being officially deprecated.

  [anti-pattern]: https://facebook.github.io/react/blog/2015/12/16/ismounted-antipattern.html

## Reference

  - This document is based on the [Airbnb React Style Guide](https://github.com/airbnb/javascript/tree/master/react).
  - All JavaScript is generally written in ES6/7 syntax adhering to the [JavaScript Standard Style](http://standardjs.com)

**[back to top](#table-of-contents)**
