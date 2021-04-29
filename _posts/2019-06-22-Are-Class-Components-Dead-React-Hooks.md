---
title: "Are Class Components Dead? - React Hooks"
date: 2019-06-22 14:20:38
categories:
  - Blog
tags: 
  - React Hooks
  - Javascript
---

Since the full release of Hooks with React version 16.8 the community has almost gone insane. Some developers have gone so bat shit crazy they’ve re-written all their apps to use React Hooks!

![](https://cdn-images-1.medium.com/max/2600/1*xcDT-neKHP7E3quS9n30gw.png)

[What are hooks](https://reactjs.org/docs/hooks-intro.html)? Plainly put, like most things JavaScript they’re just functions....simples! But with the power of scope closures they expose the beauty of Reacts state, lifecycle methods & even Context API.

You can even [write your own hooks](https://reactjs.org/docs/hooks-custom.html) if you fancy.

What kind of stuff can we do with Hooks, why should I care? Class components work just fine when I need some state & functional components are great for returning some JSX using a straight forward JS function.

But wouldn’t it be cool if you could upgrade these functional components to have their own internal state or do some processing when mounted instead of relying on the parent component to decide if it needs to be re-render or not.

Let’s start off simple & sprinkle a bit of state on the situation.

## [useState](https://reactjs.org/docs/hooks-state.html)

This hook is amazing, it works slightly different from the class based approach. Instead of managing your state in one big object, which you still can do of course. With **useState** we can create slices of state like so.

[View Demo Code](https://codesandbox.io/embed/hopeful-bassi-tvtsw)

```javascript
import React, { useState } from "react";
import ReactDOM from "react-dom";

function WhatTheState() {
  const [count, setCount] = useState(0);
  const [text, setText] = useState(null);

  return (
    <div style={{ textAlign: "center" }}>
      {text !== null ? (
        <div>
          <p>
            Hello my name is {text} & you pushed my button {count} times
          </p>
          <button onClick={() => setCount(count + 1)}>Click me</button>
        </div>
      ) : null}
      {text === null ? <p>Enter your name to start pushing!!</p> : null}
      <p>
        <input
          type="text"
          value={text}
          onChange={event => setText(event.target.value)}
        />
      </p>
    </div>
  );
}

const rootElement = document.getElementById("root");
ReactDOM.render(<WhatTheState />, rootElement);

```


Bam your functional components can now manage their own state in slices....super sweet I know. The only downside to using this approach is when you are updating your state slice you will need to supply all values otherwise they won't be included.

What’s next you ask? What about when our component mounts, checks for updates or is destroyed?

Lets control what happens when our component mounts to the DOM using the handy **useEffect** hook.

## [useEffect](https://reactjs.org/docs/hooks-effect.html)

We can manipulate or hook into the render cycles of our component now like so.

[View Code Demo](https://codesandbox.io/embed/new)

```javascript
import React, { useState, useEffect } from "react";
import ReactDOM from "react-dom";

function WhatTheState() {
  const [count, setCount] = useState(0);
  const [text, setText] = useState(null);

  useEffect(() => {
    // Just Like componentDidMount()
    setText("Johnny")
  },[])

  return (
    <div style={{ textAlign: "center" }}>
      {text !== null ? (
        <div>
          <p>
            Hello my name is {text} & you pushed my button {count} times
          </p>
          <button onClick={() => setCount(count + 1)}>Click me</button>
        </div>
      ) : null}
      {text === null ? <p>Enter your name to start pushing!!</p> : null}
      <p>
        <input
          type="text"
          value={text}
          onChange={event => setText(event.target.value)}
        />
      </p>
    </div>
  );
}

const rootElement = document.getElementById("root");
ReactDOM.render(<WhatTheState />, rootElement);

```

No need to create a stateful container anymore! Cool right? Well that’s not all we can do. We can also trigger the render based on some state change by telling the hook what to watch. Add variables to the array you'd like your component to watch & your useEffect() function will trigger again.

```javascript
useEffect(() => {
    // Similar to componentDidUpdate, we are watching for changes to the text variable
    setText("Johnny")
  },[text])
```

Last but not least we can also fire some function based on the component unmounting too. By default if your useEffect function returns a function it will invoke it when the component is unmounted.


```javascript
useEffect(() => {
    // Similar to componentDidUpdate, we are watching for changes to the text variable
    setText("Johnny")
    return function cleanup() {
      // Do some cleanup logic here!
    };
  },[text])
```

What if we're using some global state i hear you ask? Enter React's [Context API](https://reactjs.org/docs/context.html)....but wait because we're using functional components we need to use a hook instead...**useContext** hook to be exact.

## [useContext](https://reactjs.org/docs/hooks-reference.html#usecontext)

By far for me this hook is one of the best. I wouldn’t say I've had any issues with React but for a long time there was no way to manage global app state without using a third party package such as [React Redux](https://react-redux.js.org/) or [React Easy State](https://github.com/solkimicreb/react-easy-state). Passing props around to other components can be ok when it’s two or three, but when you’re passing down props through maybe 5 different components just to tell a button the app is in a loading state it becomes a bit much!

[View Demo Code](https://codesandbox.io/embed/adoring-snow-bc44k)

```javascript
import React from "react";
import ReactDOM from "react-dom";

import "./styles.css";

class App extends React.Component {
  state = {
    loading: true
  };
  render() {
    return (
      <div className="App">
        <Form loading={this.state.loading} />
      </div>
    );
  }
}

const Form = ({ loading }) => {
  return (
    <form>
      <h2>Fill Me In</h2>
      <input type="text" />
      <p>
        <Button loading={loading} />
      </p>
    </form>
  );
};

const Button = ({ loading }) => {
  if (loading) return <p>No Button Form is Loading</p>;
  return <button type="submit">Submit</button>;
};

const rootElement = document.getElementById("root");
ReactDOM.render(<App />, rootElement);
```

Do not fret my friends, **useContext** gives us access to extend our functional components. The below example illustrates how we can create some context & subscribe to it within our functional components using Reacts useContext hook.

[View Code Demo](https://codesandbox.io/embed/competent-noyce-92gz2)

```javascript
import React from "react";
import ReactDOM from "react-dom";

import "./styles.css";

const AppContext = React.createContext({ loading: "" });

class App extends React.Component {
  state = {
    loading: true
  };
  render() {
    return (
      <AppContext.Provider value={{ ...this.state }}>
        <div className="App">
          <Form loading={this.state.loading} />
        </div>
      </AppContext.Provider>
    );
  }
}

const Form = () => {
  return (
    <form>
      <h2>Fill Me In</h2>
      <input type="text" />
      <p>
        <Button />
      </p>
    </form>
  );
};

const Button = ({ loading }) => {
  const context = React.useContext(AppContext);
  if (context.loading) return <p>No Button Form is Loading</p>;
  return <button type="submit">Submit</button>;
};

const rootElement = document.getElementById("root");
ReactDOM.render(<App />, rootElement);

```

Of course you could use another state management package but I like this approach as we're relying on one package without any third party setup, no messing around with *mapStateToProps* for me anymore. Why mess with extra libraries when React provides a tool for exactly what we're trying to solve.

Side note: The size of your state will most likely determine the kind of management system you want to adopt.

Last but not least React gives us the option to extend this architecture with the **useHook** function. This is prime time for a custom hook you might have in mind. I would provide another demo but my face is tired, have a read of [this](https://reactjs.org/docs/hooks-custom.html) if you're interested.

## Conclusion

First off, React as a whole is amazing & I'm so glad I have the opportunity to develop with it every day...and get paid! Hooks are an amazing feature the React team have provided us with but understanding the core concepts of the React ecosystem & architecture in my opinion boils down to a strong grasp of stateful vs functional components.

If you can write an app using only class based stateful components go right ahead, stateful container components aren't leaving us anytime soon. Class components are a staple in React as far as understanding goes that's for sure. Hooks just give it a little extension for those pure functions that don't require a full state render method to be defined.

Need some scoped state, **useState**...need some lifecycle management / side-effects to happen **useEffect**...want to clean up your global context state **useContext**...have an idea for a reusable hook...**build your own**!

There are a couple of advanced uses regarding [useContext](https://reactjs.org/docs/hooks-reference.html#usecontext) coupled with [useReducer](https://reactjs.org/docs/hooks-reference.html#usereducer) & some [Higher Order Components](https://reactjs.org/docs/higher-order-components.html) (HOC) to completely over all your state management which you can have a read of [here](https://medium.com/@seantheurgel/react-hooks-as-state-management-usecontext-useeffect-usereducer-a75472a862fe).

Also I'd like to point out there are a couple more hooks I haven't mention in this article for example **useMemo**, **useRef** & a couple others.

Love hooks or React? Read more [React Hooks](https://reactjs.org/docs/hooks-reference.html)