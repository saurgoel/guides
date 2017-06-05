# Basics
# =======================================================
https://github.com/kriasoft/react-starter-kit
While building client-side apps, a team at Facebook reached a conclusion that a lot of web-developers had already noticed: the DOM is slow. They did, however, tackle this problem in an interesting way.
To make it faster, React implements a virtual DOM that is basically a DOM tree representation in Javascript. So when it needs to read or write to the DOM, it will use the virtual representation of it. **Then the virtual DOM will try to find the most efficient way to update the browsers DOM**.
The rationale for this is that JavaScript is very fast and its worth keeping a DOM tree in it to speedup its manipulation.
React is not a framework though. Think of it as the "View" in your traditional MVC framework.

###### Isomorphic vs Universal Javascript
React.js is the new popular guy around the "JavaScript Frameworks" block, and it shines for its simplicity. Where other frameworks implement a complete MVC framework, we could say React only implements the V (in fact, some people replace their framework's V with React). React applications are built over 2 main principles: **Components and States**. 
Components can be made of other smaller components, built-in or custom; the State drives what the guys at Facebook call one-way reactive data flow, meaning that our UI will react to every change of state.
One of the good things about React is that **it doesn't require any additional dependencies, making it pluggable with virtually any other JS library**.
It keeps a virtual React DOM which randomly updates parts that have been changed or those that need to be updated.
- React allows both client and server side rendering.
- It provides an option for pure JavaScript template creation.
- React JS components are highly re-usable.
- React comes with a small API. Beginners will find it easy to learn and start using it.
- React Native, an offshoot of React Js allows use of Javascript to write Native IOS applications.

###### Declarative (declare components)
React makes it painless to create interactive UIs. Design simple views for each state in your application, and React will efficiently update and render just the right components when your data changes.

###### Components
Build encapsulated components that manage their own state, then compose them to make complex UIs.

###### Write Anywhere
We dont make assumptions about the rest of your technology stack, so you can develop new features in React without rewriting existing code.
React can also render on the server using Node and power mobile apps using React Native.

###### Components Needed
React, Babel and Webpack

###### Reusable Components
React is built around development using components. Common react design elements can be broken down to individual components. 
React Buttons, fields, forms etc, can be saved and reused for a later time when the application developer intends to create another mobile or web UI project. 
This saves time and effort for the coder as they don’t need to write code again for the same components in the future.

###### React Router
React is only V (i.e. only view). For example backbone is VC (view and controller)
React routers have been built to bring C also.

###### React JS basics
Declarative
Unidrectional Data Flow
Composition
Explicit Mutations

###### Imperative vs Declarative
Imperative - How to do things
Declarative - What to do things

Here we are telling through the program how to do things
```
var numbers = [4,2,3]
var total = 0
for (var i=0; i < numbers.legnth; i++){
  total += numbers[i]
}

and here we are telling what to do instead of focussing on how
var numbers = [1,2,3]
numbers.reduce(function (previous, current){
  return previous + current
})
```

###### Server Side Rendering
This is a very nice feature that helps in fast rendering. Instead of waiting for the client to load javascript and then run, server itself renders the page and serves the page so that client does not need to run anything. This makes the page fast and also solves the SEO thing.
React boasts a unique feature that facilitates quick rendering of content, for better user interaction. A virtual React DOM allows both client side and server side rendering which decreases  the page load time. Facebook, a high user traffic site employs React Js since it delivers amazing user experience, which is vital for a website of its caliber.

###### Seo Benefits
I’m an Angular fan just like everybody else, but one pain point is the potential SEO impact.
But I thought Google executes and indexes javascript?
Yeah, not really. They just give you an opportunity to serve up static HTML. You still have to generate that HTML with PhantomJS or a third party service.

###### Virtual DOM
App --- Virtual DOM --- DOM
ReactJS creates its own virtual DOM where your components are kept. This approach gives developers high flexibility and amazing performance gains because ReactJS calculates what change is needed to be made in the virtual DOM in advance and updates the DOM-trees accordingly. In this fashion, ReactJS  avoids the costly DOM operations and does updates in a very efficient manner.

###### ReactJs vs AngularJS
However, ReactJS is SEO friendly, real time and compatible with heavy traffic. Whereas, AngularJS offers easy development and testing along with reliability.
React is amazing on the client side, but it’s ability to be rendered on the server side makes it truly special. This is because React uses a virtual DOM instead of the real one, and allows us to render our components to markup.

###### React vs ReactDom Library
React and ReactDOM were only recently split into two different libraries. Prior to v0.14, all ReactDOM functionality was part of React. This may be a source of confusion, since any slightly dated documentation won't mention the React / ReactDOM distinction.
As the name implies, ReactDOM is the glue between React and the DOM. Often, you will only use it for one single thing: mounting with ReactDOM.render(). Another useful feature of ReactDOM is ReactDOM.findDOMNode() which you can use to gain direct access to a DOM element. (Something you should use sparingly in React apps, but it can be necessary.) If your app is "isomorphic", you would also use ReactDOM.renderToString() in your back-end code.
The reason React and ReactDOM were split into two libraries was due to the arrival of React Native. React contains functionality utilised in web and mobile apps. ReactDOM functionality is utilised only in web apps

# JSX
# =======================================================
JSX is a preprocessor step that adds XML syntax to JavaScript. You can definitely use React without JSX but JSX makes React a lot more elegant.
Just like XML, JSX tags have a tag name, attributes, and children. If an attribute value is enclosed in quotes, the value is a string. Otherwise, wrap the value in braces and the value is the enclosed JavaScript expression.
JSX is a statically-typed, object-oriented programming language compiling to standalone JavaScript. The reason why JSX was developed is our need for a more robust programming language than JavaScript. JSX is, however, fairly close to JavaScript especially in its statements and expressions. 
Also, another important reason why JSX was developed is to boost JavaScript performance. JavaScript itself is not so slow but large-scale development tends to have many abstraction layers, e.g. proxy classes and accessor methods, which often have negative impact on performance. JSX boosts performance by inline expansion: function bodies are expanded to where they are being called, if the functions being called could be determined at compile-time. This is the power of the statically-typed language in terms of performance. 
JSX is an HTML alike syntax that compiles down to JavaScript. For JSX, markup and codes are composed in the same file. 
This means code completion gives you a helping hand as you type references to your component’s functions and variables. 
In contrast, AngularJS’s string-based templates come with the usual downsides: There is no code coloring in many editors, limited code completion support, and run-time failures. Hence ReactJS is ahead in this.

###### Installing JSX
```
npm install -g jsx
```

###### Sample JSX file
```
class _Main {
    static function main(args : string[]) : void {
        log "Hello, world!";
    }
}
```

###### Running JSX
```
jsx --run hello.jsx
```

###### Comparing JSX syntax
**In JSX**
```
<div className="red">Children Text</div>;
<MyCounter count={3 + 5} />;

// Here, we set the "scores" attribute below to a JavaScript object.
var gameScores = {
  player1: 2,
  player2: 5
};
<DashboardUnit data-index="2">
  <h1>Scores</h1>
  <Scoreboard className="results" scores={gameScores} />
</DashboardUnit>;
```
This gets transpiled into
```
React.createElement("div", { className: "red" }, "Children Text");
React.createElement(MyCounter, { count: 3 + 5 });

React.createElement(
  DashboardUnit,
  { "data-index": "2" },
  React.createElement("h1", null, "Scores"),
  React.createElement(Scoreboard, { className: "results", scores: gameScores })
);
```
You'll notice that React uses className instead of the traditional DOM class. From the docs, "Since JSX is JavaScript, identifiers such as class and for are discouraged as XML attribute names. Instead, React DOM components expect DOM property names like className and htmlFor, respectively."

###### Parsing JSX using webpack and babel
You can use .js extension and use bable-loader with react presets. It will pickup jsx automatically from js files too.
```
{ test: [/\.js$/, /\.es6$/], loader: ["babel-loader"], query: { cacheDirectory: "babel_cache", presets: ["react", "es2015"]}}
```
# COMPONENTS
# =======================================================
Component are classes. When you use them an instance of them is created with all the states, props etc.
There are two types of component. These types are not just react-based but can be visualized in any other component-based UI library or framework. They include:

**Presentation Component:** These are contained components that are responsible for UI. They are composed with JSX and rendered using the render method. The key rule about this type of component is that they are stateless meaning that no state of any sort is needed in such components. Data is kept in sync using props.
If all that a presentation component does is render HTML based on props, then you can use stateless function to define the component rather than classes.

**Container Component:** This type of component complements presentation component by providing states. Its always the guy at the top of the family tree, making sure that data is coordinated.
You do not necessarily need a state management tool outside of what React provides if what you are building does not have too much nested children and less complex. A To-Do is is simple so we can do with what React offers for now provided we understand how and when to use a presentation or container component

In React, components are the individual building blocks of how your data is viewed. You write components to handle how your data should look and to automatically render state changes. When you create a component, you define all of this by overriding **React.Component’s render function**.

###### Typechecking with propTypes
As your app grows, you can catch a lot of bugs with typechecking. For some applications, you can use JavaScript extensions like Flow or TypeScript to typecheck your whole application. But even if you don't use those, React has some built-in typechecking abilities. To run typechecking on the props for a component, you can assign the special propTypes property:

class Greeting extends React.Component {
  render() {
    return (
      <h1>Hello, {this.props.name}</h1>
    );
  }
}

Greeting.propTypes = {
  name: React.PropTypes.string
};

###### Default Prop Values
You can define default values for your props by assigning to the special defaultProps property:
```
class Greeting extends React.Component {
  render() {
    return (
      <h1>Hello, {this.props.name}</h1>
    );
  }
}

// Specifies the default values for props:
Greeting.defaultProps = {
  name: 'Stranger'
};

// Renders "Hello, Stranger":
ReactDOM.render(
  <Greeting />,
  document.getElementById('example')
);
```

###### Mixins

###### Constructor Difference
The two approaches are not interchangeable. You should initialize state in the constructor when using ES6 classes, and define the getInitialState method when using React.createClass.

See the official React doc on the subject of ES6 classes.
The difference between constructor and getInitialState is the difference between ES6 and ES5 itself.
getInitialState is used with React.createClass and
constructor is used with React.Component

```
class MyComponent extends React.Component {
  constructor(props) {
    super(props);
    this.state = { /* initial state */ };
  }
}
is equivalent to

var MyComponent = React.createClass({
  getInitialState() {
    return { /* initial state */ };
  },
});
```

n the constructor, you should always assign to this.state directly. Note that this is the only place where this is allowed. You should use this.setState() everywhere else.

**With ES6 and JSX**
```
import React from 'react';
 
class Hello extends React.Component {
  render() {
    return <h1>Hello</h1>
  }
}
```
**More shorthand**
```
export const Counter = (props) => (
  <div style={{ margin: '0 auto' }} >
    <h2>Counter: {props.counter}</h2>
    <button className='btn btn-default' onClick={props.increment}>
      Increment
    </button>
    {' '}
    <button className='btn btn-default' onClick={props.doubleAsync}>
      Double (Async)
    </button>
  </div>
)

Counter.propTypes = {
  counter     : React.PropTypes.number.isRequired,
  doubleAsync : React.PropTypes.func.isRequired,
  increment   : React.PropTypes.func.isRequired
}

export default Counter
```
**Another way to create components**
```
var Parent = React.createClass({
    componentDidMount: function() {
        console.log(this._child.someMethod()); // Prints 'bar'
    },
    render: function() {
        return (
            <div>
                <Child ref={(child) => { this._child = child; }} />
            </div>
        );
    }
});
```

First off, we have ES6 import statements and class definitions, which makes our code more concise by not having to call React.createClass. But there is also some funky looking inline HTML type stuff in the component class definition’s render function. This XML-like syntax being returned from the function is called JSX. It was designed to make building React components easier because it is concise and familiar for defining tree structures with attributes.

All of this new syntax might look a bit strange, but don’t worry because in just a bit well use Babel to transpile both the ES6 syntax and the JSX syntax into ES5 JavaScript that can be run in a browser.

**Without ES6 and JSX**
```
var React = require('react');
 
var Hello = React.createClass({displayName: 'Hello',
  render: function() {
    return React.createElement("h1", null, "Hello ");
  }
});
```

###### Rendering
Now that we have our component class, we need to add some code to “mount” this component to a DOM element. This will take our React component and render it to display within an element of an HTML page. To do this we import the React DOM and call its render function, passing in a component object as well as an actual DOM element to attach to.

**In case your components are in a module, you need to export appropriately then import it into your main js file where file is rendered.**
```
var Books = React.createClass({
	render: function() {
	return (
		<table>
			<thead>
				<tr>
					<th>Title</th>
				</tr>
			</thead>
			<tbody>
				<Book title='Professional Node.js'></Book>
				<Book title='Node.js Patterns'></Book>
			</tbody>
		</table>
		);
	}
});

var Book = React.createClass({
	render: function() {
		return (
			<tr>
				<td>{this.props.title}</td>
			</tr>
		);
	}
});

ReactDOM.render(<Books />, document.getElementById('container'));
```
Components are a very useful way to compose and reuse views (and logic).

###### Components can be Function or Class

# PROPERTIES
# =======================================================
```
var data = {
	title: 'Professional Node.js',
	author: 'Pedro Teixeira'
};
var Book = React.createClass({
	render: function() {
		return (
			<tr>
				<td>{this.props.data.title}</td>
				<td>{this.props.data.author}</td>
			</tr>
			);
		}
	});
React.render(<Book data={data}/>, document.getElementById('container'));
```
Everything in this.props is passed down to you from the parent. That includes the values that were declared in the element attributes, just like in regular HTML where you declare attributes like class or href

```
// Earlier
React.render(<Book data={data}/>, document.getElementById('container'));
// Now
ReactDOM.render(<Book data={data}/>, document.getElementById('container'));
```
###### Props are read-only
Whether you declare a component as a function or a class, it must never modify its own props. Props can be used to initiate but once renderered the following dynamicness is handled by state.
Of course, application UIs are dynamic and change over time. In the next section, we will introduce a new concept of "state". State allows React components to change their output over time in response to user actions, network responses, and anything else, without violating this rule.
Example of pure functions:
Such functions are called "pure" because they do not attempt to change their inputs, and always return the same result for the same inputs.
function sum(a, b) {
  return a + b;
}
Whereas the following changes the input and is not pure.
function withdraw(account, amount) {
  account.total -= amount;
}

# States and Lifecycle
# =======================================================
###### React Components - Function vs Class
Functions dont have a lifecycle. Give input and they give output. On the other hand react class components have a complete lifecycle.
###### Stateless Functional Components
React supports a simpler syntax called **stateless functional components** for component types like Square that only consist of a render method. Rather than define a class extending **React.Component**, **simply write a function that takes props and returns what should be rendered**:
```
function Clock(props) {
  return (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {props.date.toLocaleTimeString()}.</h2>
    </div>
  );
}
```

// A functional component using an ES2015 (ES6) arrow function:
var Aquarium = (props) => {
  var fish = getFish(props.species);
  return <Tank>{fish}</Tank>;
};

// Or with destructuring and an implicit return, simply:
var Aquarium = ({species}) => (
  <Tank>
    {getFish(species)}
  </Tank>
);

// Then use: <Aquarium species="rainbowfish" />
You can convert a functional component like Clock to a class as follows:
- Create an ES6 class with the same name that extends React.Component.
- Add a single empty method to it called render().
- Move the body of the function into the render() method.
- Replace props with this.props in the render() body.
- Delete the remaining empty function declaration.
```
class Clock extends React.Component {
  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.props.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```
You'll need to change this.props to props both times it appears. Many components in your apps will be able to be written as functional components: these components tend to be easier to write and React will optimize them more in the future.

###### Adding Local state to Class
Here we will be adding a class constructor.
Props are generally used in functional elements. Don't use props if the component is a lifecycle component i.e. the value changes via the lifecycle of the component. For this use states. Here 
```
class Clock extends React.Component {
  constructor(props) {
	// You need to pass props
    super(props);
    this.state = {date: new Date()};
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```

###### Adding lifecycle methods to a class
**componentDidMount** - We want to set up a timer whenever the Clock is rendered to the DOM for the first time. This is called "mounting" in React.
**componentWillUnmount** - We also want to clear that timer whenever the DOM produced by the Clock is removed. This is called "unmounting" in React.
These methods are called "lifecycle hooks".
The componentDidMount() hook runs after the component output has been rendered to the DOM. This is a good place to set up a timer.
**Other Fields** - While this.props is set up by React itself and this.state has a special meaning, you are free to add additional fields to the class manually if you need to store something that is not used for the visual output.
```
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  componentDidMount() {
    this.timerID = setInterval(
      () => this.tick(),
      1000
    );
  }

  componentWillUnmount() {
    clearInterval(this.timerID);
  }

  tick() {
    this.setState({
      date: new Date()
    });
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}

ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```

- When <Clock /> is passed to ReactDOM.render(), React calls the constructor of the Clock component. Since Clock needs to display the current time, it initializes this.state with an object including the current time. We will later update this state.
- React then calls the Clock component's render() method. This is how React learns what should be displayed on the screen. React then updates the DOM to match the Clock's render output.
- When the Clock output is inserted in the DOM, React calls the componentDidMount() lifecycle hook. Inside it, the Clock component asks the browser to set up a timer to call tick() once a second.
- Every second the browser calls the tick() method. Inside it, the Clock component schedules a UI update by calling setState() with an object containing the current time. Thanks to the setState() call, React knows the state has changed, and calls render() method again to learn what should be on the screen. This time, this.state.date in the render() method will be different, and so the render output will include the updated time. React updates the DOM accordingly.
- If the Clock component is ever removed from the DOM, React calls the componentWillUnmount() lifecycle hook so the timer is stopped.

###### Use setState
Do not modify state directly. For example, this will not re-render a component:
// Wrong
this.state.comment = 'Hello';
Instead, use setState():
// Correct
this.setState({comment: 'Hello'});

###### State updates may be asyncronous
React may batch multiple setState() calls into a single update for performance.
Because this.props and this.state may be updated asynchronously, you should not rely on their values for calculating the next state.
For example, this code may fail to update the counter:
```
// Wrong
this.setState({
  counter: this.state.counter + this.props.increment,
});
```
To fix it, use a second form of setState() that accepts a function rather than an object. That function will receive the previous state as the first argument, and the props at the time the update is applied as the second argument:
```
// Correct
this.setState((prevState, props) => ({
  counter: prevState.counter + props.increment
}));
```
We used an arrow function above, but it also works with regular functions:
```
this.setState(function(prevState, props) {
  return {
    counter: prevState.counter + props.increment
  };
});
```

###### State Updates are merged
When you call setState(), React merges the object you provide into the current state.

For example, your state may contain several independent variables:
```
  constructor(props) {
    super(props);
    this.state = {
      posts: [],
      comments: []
    };
  }
```
Then you can update them independently with separate setState() calls:
```
  componentDidMount() {
    fetchPosts().then(response => {
      this.setState({
        posts: response.posts
      });
    });

    fetchComments().then(response => {
      this.setState({
        comments: response.comments
      });
    });
  }
```
The merging is shallow, so this.setState({comments}) leaves this.state.posts intact, but completely replaces this.state.comments.

###### Playing between components
Neither parent nor child components can know if a certain component is stateful or stateless, and they shouldn't care whether it is defined as a function or a class.
This is why state is often called local or encapsulated. It is not accessible to any component other than the one that owns and sets it.

A component may choose to pass its state down as props to its child components:
<FormattedDate date={this.state.date} />
The FormattedDate component would receive the date in its props and wouldn't know whether it came from the Clock's state, the props, or was typed by hand:

function FormattedDate(props) {
  return <h2>It is {props.date.toLocaleTimeString()}.</h2>;
}

This is commonly called a "top-down" or "unidirectional" data flow. Any state is always owned by some specific component, and any data or UI derived from that state can only affect components "below" them in the tree.

###### Lifting State Up
Often, several components need to reflect the same changing data. We recommend lifting the shared state up to their closest common ancestor. Let's see how this works in action.



# EVENTS
# =======================================================
React events are named using camelCase, rather than lowercase.
With JSX you pass a function as the event handler, rather than a string.
```
<button onclick="activateLasers()">
  Activate Lasers
</button>
```
is slightly different in React:

```
<button onClick={activateLasers}>
  Activate Lasers
</button>
```

Another difference is that you cannot return false to prevent default behavior in React. You must call preventDefault explicitly. For example, with plain HTML, to prevent the default link behavior of opening a new page, you can write:
```
<a href="#" onclick="console.log('The link was clicked.'); return false">
  Click me
</a>
```
In React, this could instead be:
```
function ActionLink() {
  function handleClick(e) {
    e.preventDefault();
    console.log('The link was clicked.');
  }

  return (
    <a href="#" onClick={handleClick}>
      Click me
    </a>
  );
}
```


# STATE
# =======================================================
**https://medium.com/@firasd/quick-start-tutorial-universal-react-with-server-side-rendering-76fe5363d6e#.edxu285rm
https://medium.com/@firasd/quick-start-tutorial-using-redux-in-react-apps-89b142d6c5c1#.avk3yuh9o**

React components can have state by setting this.state in the constructor, which should be considered private to the component. Let's store the current value of the square in state, and change it when the square is clicked. First, add a constructor to the class to initialize the state:

Properties vs State
```
class Square extends React.Component {
  constructor() {
    super();
    this.state = {
      value: null,
    };
  }
  ...
}
```

An interactive component
```
<button className="square" onClick={() => alert('click')}>
```

In JavaScript classes, **you need to explicitly call super(); when defining the constructor of a subclass.**

Now change the render method to display this.state.value instead of this.props.value, and change the event handler to be () => this.setState({value: 'X'}) instead of the alert:

```
<button className="square" onClick={() => this.setState({value: 'X'})}>
    {this.state.value}
</button>

<button className="square" onClick={() => this.props.onClick()}>

class Board extends React.Component {
  constructor() {
    super();
    this.state = {
      squares: Array(9).fill(null),
    };
  }
}

renderSquare(i) {
  return <Square value={this.state.squares[i]} />;
}

renderSquare(i) {
  return <Square value={this.state.squares[i]} onClick={() => this.handleClick(i)} />;
}
```




Props are not meant to be changed 
They are passed at the start of the component in the constructur 
post that they remain the same 
for chaning vars use state
Pages that require auth to render?


# STATE MANAGEMENT - FLUX
# =======================================================


# STATE MANAGEMENT - REDUX
# =======================================================
At some point in your complex React project, you are going to need a state management library. Redux is a good choice because of it’ss simplicity and centralized data management. This piece is a practical approach to the fundamentals of Redux in building React application for managing a book store.
As web applications grow in complexity, so does the task of updating and displaying their underlying data. Many approaches to managing this data result in a complex web of views. These views may be listening for updates from various models which are pushing their changes to yet more views. This leaves developers with opaque, non-deterministic code that is nearly impossible to change without forgetting to attach a listener to an important strand in the web. Even worse, developers could introduce a bug in a different, seemingly unconnected corner of the application. Enter, Redux! Its a predictable state container for JavaScript apps that offers a solution to this problem.

concepts of redux:
There is a single source of truth for your entire application state
That state is read-only
All changes to the application state are made by pure functions

When you use Redux, the underlying data for your entire application is represented by a single JavaScript object, referred to as the state or state tree. This object can be as simple or complex as your application demands. For example, the state for a simple todo app might be a single array of todo objects.
```
const state = [
    {
        id: 1,
        task: 'Do laundry',
        completed: true
    },
    {
        id: 2,
        task: 'Paint fence',
        completed: false
    }
];
```

The state for a social media site might be a dictionary that contains information about posts, notifications, profile data, and other social data.
Regardless of the size of the application, all state data is stored in a single object. 
```
const defaultState = {
    posts: [
        // post objects to appear in users feed
    ],
    notifications: [
        // unread notifications for the user
    ],
    messages: [
        // new messages
    ],
    friends: [
        // other online users
    ],
    profile: null
}
```


# EVENT ARCHITECTURES BASED ON REACT
# =======================================================
There are lots of architectures based on react.

###### Flux Architecture
Flux is the application architecture that Facebook uses for building client-side web applications. It complements Reacts composable view components by utilizing a unidirectional data flow.
Its more of a pattern rather than a formal framework, and you can start using Flux immediately without a lot of new code.

###### Redux Architecture
Redux is a predictable state container for JavaScript apps
“Its cool that you are inventing a better Flux by not doing Flux at all.”
Redux evolves the ideas of Flux, but avoids its complexity by taking cues from Elm.

###### Install stable version

###### Flux and Other architectures
First of all, it is totally possible to write apps with React without Flux
I believe you should start with pure React, than learn Redux and Flux. After you will have some REAL experience with React, you will see whether Redux is helpfull for you or not.
If you start directly with Redux, you may end up with over-engineered code, code hadrer to maintain and with even more bugs and than without Redux.
Flux is a general framework for laying out how data flows and state changes in your application. Redux is one implementation (the best, in my opinion) of the concept of flux.
They are both conceptually independent of React, although the top-down data flow that flux enables is very complimentary to how React prefers that data flows.
So, to answer your question, learn Redux with React.


# React API Calls
# =======================================================
Api call libraries
Superagent
Fetch
Jquery

###### Root Component
When you start asking about AJAX and React, the first thing the experts will tell you is that React is a view library and React has no networking/AJAX features.
http://andrewhfarmer.com/react-ajax-best-practices/
Do not use jQuery to send AJAX requests. jQuery is a large library with many features, so using it just for AJAX doesn't make sense.
I recommend using fetch(). It is a simple, standardized, JavaScript AJAX API. It is already supported by Chrome and Firefox, and polyfills are available for node and other browsers.

When to use root component
You have a shallow component tree.
You're not using Redux or flux.

###### Container Components
A container component "provides the data and behavior to presentational or other container components."
- You have a deep component tree.
- Many of your components do not require data from the server, but some do.
- You are fetching data from multiple APIs or endpoints.
- You're not using Redux/flux, or you prefer container components to 'async actions'.

###### Redux Async Actions

###### Relay

# Thinking in React
# =======================================================
https://facebook.github.io/react/docs/thinking-in-react.html
Since you're often displaying a JSON data model to a user, you'll find that if your model was built correctly, your UI (and therefore your component structure) will map nicely. That's because UI and data models tend to adhere to the same information architecture, which means the work of separating your UI into components is often trivial. Just break it up into components that represent exactly one piece of your data model.
- FilterableProductTable
  - SearchBar
  - ProductTable
    - ProductCategoryRow
    - ProductRow

# Props vs State
Props and states

Global States and Local States

# Redux Global State vs Local State
don't use Redux until you have problems with vanilla React.
You'll know when you need Flux. If you aren't sure if you need it, you don't need it.

use Redux when you have reasonable amounts of data changing over time, you need a single source of truth, and you find that approaches like keeping everything in a top-level React component's state are no longer sufficient.

http://redux.js.org/docs/faq/General.html#general-when-to-use

I've been working with Redux to manage component state and have found it's a lot more work than just using this.state. A lot for the time there doesn't seem to be much of a payoff except when you want to share that state with other components. The zealots of the "One true flow" seem to never make exceptions: Everything must got through the Action > Reducer > Component cycle which is laborious to implement (especially of your doing testing).

I've come to see Redux as global state and this.state as local state, as one sees global and local variables, but nobody seems to talk about this or come up with rules-of-thumb as to weather using an mix of both as permissible and if it is when is it OK?

For example i'm finding that with controlled forms, in which the state is never needed outside the component until after it is submitted, its faster and easier to just use local state and update Redux after the submitted data has changed something on the server.

My rule of thumb is now: If the data is not referenced outside the component then use local state. If its share between components use global (Redux).

Is there some rationale as to why this would be a misnomer? Arguments for or against?
You are absolutely correct, I use local state to keep track of form data, boolean values for conditional rendering, and anything else the component might need to do its thing. This is especially true if you have a prop on your component from the store, and you allow the user to edit that prop. if you set it on the state in getInitialState() { return { user: this.props.user} } , cancelling the edit is as easy as calling this.setState(this.getInitialState()) (assuming the user prop is an immutable object)

However, the issue is, if the component gets unmounted for whatever reason (route change usually) then you lose the data.

# Stateful Component

# Conditional Rendering
# =======================================================
Consider we have 2 components
```
function UserGreeting(props) {
  return <h1>Welcome back!</h1>;
}
function GuestGreeting(props) {
  return <h1>Please sign up.</h1>;
}
```

And we can print these depending on the state
```
function Greeting(props) {
  const isLoggedIn = props.isLoggedIn;
  if (isLoggedIn) {
    return <UserGreeting />;
  }
  return <GuestGreeting />;
}
ReactDOM.render(
  // Try changing to isLoggedIn={true}:
  <Greeting isLoggedIn={false} />,
  document.getElementById('root')
);
```


```
function LoginButton(props) {
  return (
    <button onClick={props.onClick}>
      Login
    </button>
  );
}
function LogoutButton(props) {
  return (
    <button onClick={props.onClick}>
      Logout
    </button>
  );
}
```

Full Login Control Code
```
class LoginControl extends React.Component {
  constructor(props) {
    super(props);
    this.handleLoginClick = this.handleLoginClick.bind(this);
    this.handleLogoutClick = this.handleLogoutClick.bind(this);
    this.state = {isLoggedIn: false};
  }

  handleLoginClick() {
    this.setState({isLoggedIn: true});
  }

  handleLogoutClick() {
    this.setState({isLoggedIn: false});
  }

  render() {
    const isLoggedIn = this.state.isLoggedIn;

    let button = null;
    if (isLoggedIn) {
      button = <LogoutButton onClick={this.handleLogoutClick} />;
    } else {
      button = <LoginButton onClick={this.handleLoginClick} />;
    }

    return (
      <div>
        <Greeting isLoggedIn={isLoggedIn} />
        {button}
      </div>
    );
  }
}

ReactDOM.render(
  <LoginControl />,
  document.getElementById('root')
);
```

Another method for conditionally rendering elements inline is to use the JavaScript conditional operator condition ? true : false.

###### Inline Conditions
You may embed any expressions in JSX by wrapping them in curly braces. This includes the JavaScript logical && operator. It can be handy for conditionally including an element:
**true && expression** always evaluates to expression, and false && expression always evaluates to false
```
function Mailbox(props) {
  const unreadMessages = props.unreadMessages;
  return (
    <div>
      <h1>Hello!</h1>
      {unreadMessages.length > 0 &&
        <h2>
          You have {unreadMessages.length} unread messages.
        </h2>
      }
    </div>
  );
}

const messages = ['React', 'Re: React', 'Re:Re: React'];
ReactDOM.render(
  <Mailbox unreadMessages={messages} />,
  document.getElementById('root')
);
```

In the example below, we use it to conditionally render a small block of text.
**conditional operator condition ? true : false**
```
render() {
  const isLoggedIn = this.state.isLoggedIn;
  return (
    <div>
      The user is <b>{isLoggedIn ? 'currently' : 'not'}</b> logged in.
    </div>
  );
}
```
It can also be used for larger expressions although it is less obvious what's going on:

```
render() {
  const isLoggedIn = this.state.isLoggedIn;
  return (
    <div>
      {isLoggedIn ? (
        <LogoutButton onClick={this.handleLogoutClick} />
      ) : (
        <LoginButton onClick={this.handleLoginClick} />
      )}
    </div>
  );
}
```


###### Inline Traversing
```
function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) =>
    <li>{number}</li>
  );
  return (
    <ul>{listItems}</ul>
  );
}

function Blog(props) {
  const sidebar = (
    <ul>
      {props.posts.map((post) =>
        <li key={post.id}>
          {post.title}
        </li>
      )}
    </ul>
  );
  const content = props.posts.map((post) =>
    <div key={post.id}>
      <h3>{post.title}</h3>
      <p>{post.content}</p>
    </div>
  );
  return (
    <div>
      {sidebar}
      <hr />
      {content}
    </div>
  );
}
```


# SETTING UP REACT ON EXPRESS 1
# =======================================================
###### Creating a node app
```
npm init
```

###### Install react and react-dom
```
npm install --save react
npm install --save react-dom
```

###### Install Webpack
We will also need to install Webpack and the Webpack development server for serving our bundled JavaScript application. You may need to use “sudo” to install the dev server package globally.
```
npm install --save-dev webpack
npm install webpack-dev-server -g
```

###### Install Babel
Now that our bundling tool is taken care of, we need a transpiler for interpreting our ES6 code. This is where Babel comes in. Let’s install the babel-loader and babel-core packages that we’ll use to work with Webpack, as well as the ES2015 and React presets for loading the code that we’ll write.
```
npm install --save-dev babel-loader
npm install --save-dev babel-core
npm install --save-dev babel-preset-es2015
npm install --save-dev babel-preset-react
```

###### Creating a react component
**hello.jsx**
```
import React from 'react';
import ReactDOM from 'react-dom';
 
class Hello extends React.Component {
  render() {
    return <h1>Hello</h1>
  }
}
ReactDOM.render(<Hello/>, document.getElementById('hello'));
```
**world.jsx**
```
import React from 'react';
import ReactDOM from 'react-dom';
 
class World extends React.Component {
  render() {
    return <h1>World</h1>
  }
}
 
ReactDOM.render(<World/>, document.getElementById('world'));
```
Adding an index.html file. The JS will render hello world here using react.
**index.html**
```
<!doctype html>
<html>
  <head>
    <meta charset="UTF-8">
    <title>Hello React</title>
  </head>
  <body>
    <div id="hello"></div>
    <div id="world"></div>
  </body>
</html>
```

Another Example
**hello.js**
```
import React from "react";

export default React.createClass({
 render: function() {
   return (
     <div>
         Hello, {this.props.name}!
     </div>
   );
 },
});
```
**app.js**
```
import React from "react";
import ReactDOM from "react-dom";
import Hello from "./hello";

ReactDOM.render(
  <Hello name="World" />,
  document.body
);
```
###### Bundling with webpack
Now that we have the 2 JSX files ready we want to serve the final JS file using webpack. For this we first merge the 2 JSX files in main.js file and then supply this to webpack, which will do the bundling for us.

**main.js**
```
import Hello from './hello.jsx';
import World from './world.jsx';
```
Following is the webpack config. Currently we are using only the babel loader but we can use other loaders like sass, coffeescript loader as well if we want to do other transformations.
```
var path = require('path');
var webpack = require('webpack');
 
module.exports = {
  entry: './main.js',
  output: { path: __dirname, filename: 'bundle.js' },
  module: {
    loaders: [
      {
        test: /.jsx?$/,
        loader: 'babel-loader',
        exclude: /node_modules/,
        query: {
          presets: ['es2015', 'react']
        }
      }
    ]
  },
};
```

Including the final file in html
```
<!doctype html>
<html>
  <head>
    <meta charset="UTF-8">
    <title>Hello React</title>
  </head>
  <body>
    <div id="hello"></div>
    <div id="world"></div>
    <script src="bundle.js"></script>
  </body>
</html>
```

###### Serving assets using webpack
Now start the webpack-dev-server using the following command:
```
webpack-dev-server --progress --colors
```


# SETTING UP REACT WITH EXPRESS 2
# =======================================================
**Module sharing**: how to use Node.js modules also in the browser.
**Universal rendering**: how to render the views of the application from the server (during the initialization of the app) and then keep rendering the other views directly in the browser (avoiding a full page refresh) while the user keep navigating across the different sections.
**Universal routing**: how to recognize the view associated to the current route from both the server and the browser.
**Universal data retrival**: how to access data (typically through APIs) from both the server and the browser.


We need npm to install all the stuff
We will need to have babel, ejs, express, react and react-router installed. To do so you can run the following command:
```
npm install --save babel-cli@6.11.x babel-core@6.13.x  babel-preset-es2015@6.13.x babel-preset-react@6.11.x ejs@2.5.x express@4.14.x react@15.3.x react-dom@15.3.x react-router@2.6.x
```

We will also need to install Webpack(with its Babel loader extension) and http-server as a development dependencies:
```
npm install --save-dev webpack@1.13.x babel-loader@6.2.x http-server@0.9.x
```
File Structure
```
├── package.json
├── webpack.config.js
├── src
│   ├── app-client.js
│   ├── routes.js
│   ├── server.js
│   ├── components
│   │   ├── AppRoutes.js
│   │   ├── AthletePage.js
│   │   ├── AthletePreview.js
│   │   ├── AthletesMenu.js
│   │   ├── Flag.js
│   │   ├── IndexPage.js
│   │   ├── Layout.js
│   │   ├── Medal.js
│   │   └── NotFoundPage.js
│   ├── data
│   │   └── athletes.js
│   ├── static
│   │   ├── index.html
│   │   ├── css
│   │   ├── favicon.ico
│   │   ├── img
│   │   └── js
│   └── views
        └──  index.ejs
```

###### Basic View
create and add stuff in src/static/index.html
```
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Judo Heroes - A Universal JavaScript demo application with React</title>
<link rel="stylesheet" href="/css/style.css">
</head>
<body>
<div id="main"></div>
<script src="/js/bundle.js"></script>
</body>
</html>
```

###### Javascript Data Module
Currently just for the sake we are importing data syncornously which otherwise we would have done asynchonously using an API call
**src/data/athletes.js**
```
const athletes = [
{
'id': 'driulis-gonzalez',
'name': 'Driulis González',
'country': 'cu',
'birth': '1973',
'image': 'driulis-gonzalez.jpg',
'cover': 'driulis-gonzalez-cover.jpg',
'link': 'https://en.wikipedia.org/wiki/Driulis_González',
'medals': [
{ 'year': '1992', 'type': 'B', 'city': 'Barcelona', 'event': 'Olympic Games', 'category': '-57kg' },
{ 'year': '1993', 'type': 'B', 'city': 'Hamilton', 'event': 'World Championships', 'category': '-57kg' },
{ 'year': '1995', 'type': 'G', 'city': 'Chiba', 'event': 'World Championships', 'category': '-57kg' },
{ 'year': '1995', 'type': 'G', 'city': 'Mar del Plata', 'event': 'Pan American Games', 'category': '-57kg' },
{ 'year': '1996', 'type': 'G', 'city': 'Atlanta', 'event': 'Olympic Games', 'category': '-57kg' },
{ 'year': '1997', 'type': 'S', 'city': 'Osaka', 'event': 'World Championships', 'category': '-57kg' },
{ 'year': '1999', 'type': 'G', 'city': 'Birmingham', 'event': 'World Championships', 'category': '-57kg' },
{ 'year': '2000', 'type': 'S', 'city': 'Sydney', 'event': 'Olympic Games', 'category': '-57kg' },
{ 'year': '2003', 'type': 'G', 'city': 'S Domingo', 'event': 'Pan American Games', 'category': '-63kg' },
{ 'year': '2003', 'type': 'S', 'city': 'Osaka', 'event': 'World Championships', 'category': '-63kg' },
{ 'year': '2004', 'type': 'B', 'city': 'Athens', 'event': 'Olympic Games', 'category': '-63kg' },
{ 'year': '2005', 'type': 'B', 'city': 'Cairo', 'event': 'World Championships', 'category': '-63kg' },
{ 'year': '2006', 'type': 'G', 'city': 'Cartagena', 'event': 'Central American and Caribbean Games', 'category': '-63kg' },
{ 'year': '2006', 'type': 'G', 'city': 'Cartagena', 'event': 'Central American and Caribbean Games', 'category': 'Tema' },
{ 'year': '2007', 'type': 'G', 'city': 'Rio de Janeiro', 'event': 'Pan American Games', 'category': '-63kg' },
{ 'year': '2007', 'type': 'G', 'city': 'Rio de Janeiro', 'event': 'World Championships', 'category': '-63kg' },
],
}
];
export default athletes;
```

###### Application Routes
**Routes module** - In this file we are basically using the React Router Route component to map a set of routes to the page components we defined before. Note how the routes are nested inside a main Route component.
**src/routes.js**
```
import React from 'react'
import { Route, IndexRoute } from 'react-router'
import Layout from './components/Layout';
import IndexPage from './components/IndexPage';
import AthletePage from './components/AthletePage';
import NotFoundPage from './components/NotFoundPage';

const routes = (
<Route path="/" component={Layout}>
<IndexRoute component={IndexPage}/>
<Route path="athlete/:id" component={AthletePage}/>
<Route path="*" component={NotFoundPage}/>
</Route>
);
export default routes;
```

**src/components/AppRoutes.js**
```
import React from 'react';
import { Router, browserHistory } from 'react-router';
import routes from '../routes';

export default class AppRoutes extends React.Component {
  render() {
    return (
      <Router history={browserHistory} routes={routes} onUpdate={() => window.scrollTo(0, 0)}/>
    );
  }
}
```

Here we need to import the Router component from react-router and then reurn it with the following:
**browserHistory** - configure the history prop to specify that we want to use the HTML5 browser history for the routing
routes from our routes directory
added an onUpdate callback to reset the scrolling of the window to the top everytime a link is clicked.

###### Application entry point
**src/app-client.js**
```
import React from 'react';
import ReactDOM from 'react-dom';
import AppRoutes from './components/AppRoutes';

window.onload = () => {
  ReactDOM.render(<AppRoutes/>, document.getElementById('main'));
};
```

###### Setting Webpack and Babel
Webpack is a module bundler. Webpack will convert ES2015 and React JSX syntax to equivalent ES5 syntax (using Babel), which can be executed practically by every browser.
Furthermore we can use Webpack to apply a number of optimizations to the resulting code like combining all the scripts files into one file and minifying the resulting bundle.

###### Webpack Config
**webpack.config.js**
```
const webpack = require('webpack');
const path = require('path');

module.exports = {
  entry: path.join(__dirname, 'src', 'app-client.js'),
  output: {
    path: path.join(__dirname, 'src', 'static', 'js'),
    filename: 'bundle.js'
  },
  module: {
    loaders: [{
      test: path.join(__dirname, 'src'),
      loader: ['babel-loader'],
      query: {
        cacheDirectory: 'babel_cache',
        presets: ['react', 'es2015']
      }
    }]
  },
  plugins: [
    new webpack.DefinePlugin({
      'process.env.NODE_ENV': JSON.stringify(process.env.NODE_ENV)
    }),
    new webpack.optimize.DedupePlugin(),
    new webpack.optimize.OccurenceOrderPlugin(),
    new webpack.optimize.UglifyJsPlugin({
      compress: { warnings: false },
      mangle: true,
      sourcemap: false,
      beautify: false,
      dead_code: true
    })
  ]
};
```

###### Basic config:
In the first part of the configuration file we define what is the entry point and the otput file. The entry point is the main JavaScript file that initializes the application. Webpack will recursively resolve all the included/imported resources to determine which files will go into the final bundle.

###### Modules:
The module.loaders section allows to specify transformations on specific files. Here we want to use Babel with the react and es2015 presets to convert all the included JavaScript files to ES5 code.

###### Plugins used:
DefinePlugin allows us to define the NODE_ENV variable as a global variable in the bundling process as if it was defined in one of the scripts. 
DedupePlugin removes all the duplicated files (modules imported in more than one module).
OccurenceOrderPlugin helps in reducing the file size of the resulting bundle.
UglifyJsPlugin minifies and obfuscates the resulting bundle using UglifyJs.

###### Generate bundle file
The NODE_ENV environment variable and the -p option are used to generate the bundle in production mode, which will apply a number of additional optimizations, for example removing all the debug code from the React library.
```
NODE_ENV=production node_modules/.bin/webpack -p
```

###### Creating server
We dont have a Node.js web server yet, so for now we can just use the module http-server (previously installed as development dependency) to run a simple static file web server:
```
node_modules/.bin/http-server src/static
```

###### Adding Universal Rendering
Server file for expressJS
**src/server.js**
```
import path from 'path';
import { Server } from 'http';
import Express from 'express';
import React from 'react';
import { renderToString } from 'react-dom/server';
import { match, RouterContext } from 'react-router';
import routes from './routes';
import NotFoundPage from './components/NotFoundPage';
```

###### Initialize the server and configure support for ejs templates
```
const app = new Express();
const server = new Server(app);
app.set('view engine', 'ejs');
app.set('views', path.join(__dirname, 'views'));
```

###### Define the folder that will be used for static assets
```
app.use(Express.static(path.join(__dirname, 'static')));
```

###### Universal routing and rendering
```
app.get('*', (req, res) => {
  match(
    { routes, location: req.url },
    (err, redirectLocation, renderProps) => {

      // in case of error display the error message
      if (err) {
        return res.status(500).send(err.message);
      }

      // in case of redirect propagate the redirect to the browser
      if (redirectLocation) {
        return res.redirect(302, redirectLocation.pathname + redirectLocation.search);
      }

      // generate the React markup for the current route
      let markup;
      if (renderProps) {
        // if the current route matched we have renderProps
        markup = renderToString(<RouterContext {...renderProps}/>);
      } else {
        // otherwise we can render a 404 page
        markup = renderToString(<NotFoundPage/>);
        res.status(404);
      }

      // render the index template with the embedded React markup
      return res.render('index', { markup });
    }
  );
});
```

// start the server
```
const port = process.env.PORT || 3000;
const env = process.env.NODE_ENV || 'production';
server.listen(port, err => {
  if (err) {
    return console.error(err);
  }
  console.info(Server running on http://localhost:${port} [${env}]);
});
```

https://medium.com/engineering-housing/progressing-mobile-web-fac3efb8b454#.pj1tjx4cm
React-Forms

# React-Router
import { IndexLink, Link } from "react-router"


Other functionaliesi



# Glossaary
# =======================================================
Table of Contents
Foreword
Why React?
Who should read this book?
JSX
Learning React with ES6 and JSX
General Installation Instructions
Hands-on: Hello world in React — step by step
React.DOM.tagName(properties, children)
React.renderComponent(what, where) — your main() function for React
What you already learnt
Nested Html — Let’s go with 2 tags
Responding to click events
Changing the content manually
Component
Q&A
Moving on
Hands-on: Create new meetup form
Simple beginning
Debugging
Eliminating duplication with a custom component
Component default properties
Sending form data
Sending form data with Ajax
Meetup date
SEO Text
Title validation
All validations checked on submission
Inviting guests by email
Box components
Selecting meetup technology
Mastering Components
Components — the main building block
Anatomy of the React component
State
Properties
Component children
render method
Mixins
Context
References (refs) to components
JSX
Bonus: Syntax comparison of React Components
Integration with external world
Forms
Event Handlers
Lifecycle Methods
React.findDOMNode
Passing state
Root component approach
Passing state through references
More
Animations
React.addons.CSSTransitionGroup
Flash notifications example
No need to animate every time
Integrating components with browser animations
Worth reading and watching
Testing
Set up your test environment within Rails
Testing through getDOMNode
Testing through simulating events
Testing with references
scry/find family of functions
Tips: Removing duplication in handlers
Use bind
Publish custom events from your own component
Tips: setState quirks
Overcoming the limitations
Tips: State outside React
Introduction
How
Summary
Bonus content:

From Rails partial to React components
First React component
Prepare data for your Single Page App
Placeholder where React component will be rendered
Initialize the root component with JSON embedded into HTML
What we learned in this small comparison example?
Moving helpers to React
Initialize the app with JSON via HTTP
Next
Say hello to state
React.js and dynamic children — Why the Keys are Important
Problem: Component doesn’t have initial state defined but the previous one
React will determine whether it is the same component or not based on key
New component on the same level as old one? Would you kindly change a key?
What did I just learn?
Now go home and…
Use your gettext translations in your React components
Transitioning from backend to frontend
i18next — move your gettext to frontend
React.js and Google Charts
Gulp — a modern approach to asset pipeline for Rails developers
Why Gulp?
Let’s start: CoffeeScript with Browserify and Sass
Creating the Rails app without Sprockets
Gulpfile.js
Compiling Sass
A note about streams
Adding source maps
Compiling CoffeeScript
Watching for changes
More?
Summary
CoffeeScript is the fresh air
Frontend is a separate application
From backend to frontend — the mental transition
JS frontends are like desktop apps
Is CoffeeScript better than Ruby?
Frontend first
Building a React.js event log in a Rails admin panel
Example
Backend part
Frontend part
What next?
Our other posts related to ES topic
Start using ES6 with Rails today
How can I use ES6 in my web browser?
Bringing ES6 to Rails
Conclusions
Beautiful confirm window with React
Getting started
Modal window
Confirm modal
Making it work
Summary
On my radar: RethinkDB + React.js + Rails
Final effect
Dragons
Final thoughts
Upgrading React from 0.11 to 0.13
API changes
Upgrade to 0.12 first
Terminology changes
Transform your components to elements
Get rid of getDOMNode
classSet addon becomes deprecated
Example: Transforming create new meetup form example to be 0.13 ready
Disclaimer: Do not wrap your createClass in createFactory!
Better way: Leverage CommonJS to simplify wrap-with-factory process



# React on Rails
# ===================================================
Like the react-rails gem, React on Rails is capable of server-side rendering with fragment caching and is compatible with turbolinks. Unlike react-rails, which depends heavily on sprockets and jquery-ujs, React on Rails uses webpack and does not depend on jQuery. 
- Redux
- Webpack optimization functionality
- React Router

Webpack is used to generate several JavaScript "bundles" for inclusion in application.js or directly in your layout.
This usage of webpack fits neatly and simply into the existing Rails sprockets system and you can include React components on a Rails view with a simple helper.
Other apps will use separate node server to distribute assets and rails app for only apis.

###### react-rails
react-rails makes it easy to use React and JSX in your Ruby on Rails (3.2+) application.
Provide various react builds to your asset bundle
Transform .jsx in the asset pipeline
Render components into views and mount them via view helper & react_ujs
- Render components server-side with prerender: true
- Generate components with a Rails generator
- Be extended with custom renderers, transformers and view helpers

###### react on rails
Instead of using sprockets it uses webpack for rendering the server assets. Webpack etc is included separately using Procfiles. There are npm modules etc. also present inside, and packages are installed via npm inside the rails directory.
Like the react-rails gem, React on Rails is capable of server-side rendering with fragment caching and is compatible with turbolinks. Unlike react-rails, which depends heavily on sprockets and jquery-ujs, React on Rails uses webpack and does not depend on jQuery.
- Redux
- Webpack optimization functionality
- React Router

Webpack is used to generate several JavaScript "bundles" for inclusion in application.js or directly in your layout.
This usage of webpack fits neatly and simply into the existing Rails sprockets system and you can include React components on a Rails view with a simple helper.
Other apps will use separate node server to distribute assets and rails app for only apis.

###### Links
https://github.com/reactjs/react-rails
http://blog.arkency.com/rails-react/

react-rails makes it easy to use React and JSX in your Ruby on Rails (3.2+) application.
Provide various react builds to your asset bundle
Transform .jsx in the asset pipeline
Render components into views and mount them via view helper & react_ujs
- Render components server-side with prerender: true
- Generate components with a Rails generator
- Be extended with custom renderers, transformers and view helpers

###### More Components
react_on_rails Gem: Webpack Integration of React with Rails utilizing the modern JavaScript tooling and libraries, including Webpack, Babel, React, Redux, React-Router. You can an example of this live at www.reactrails.com.
React.rb: Use Ruby to build reactive user interfaces with React under the covers.github source code here.
react-rails-hot-loader is a simple live-reloader for react-rails.
react-rails-benchmark_renderer adds performance instrumentation to server rendering.