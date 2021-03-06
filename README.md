# Needs Review 
- JSX .map function
- JSX key(s) mapping

# notes-on-react
Notes on my adventure into the world of React, React-Native, and Redux beginning on July 30th, 2016
***
## ReactDOM
#### ReactDOM.render is the most common way to render JSX
	1. It takes a JSX expression
	2. Creates a corresponding tree of DOM nodes
	3. Adds that tree to the DOM
```
ReactDOM.render(
	argument which must evaluate to JSX,
	domContainerNode which usually is getElementBy..
)
```
* ReactDOM only updates things that have changed
* If the same thing is rendered twice, the __second render function will do nothing__
***
## Components & Component Class (Component != Component Class)
Component => A small, reusable chunk of code that is responsible for one job. That job is often to render some HTML.
- A component takes two optional inputs, PROPS and STATE, and outputs a piece of UI
- Every component must come from a `Component Class` (Please see below)
  - Component Class => Factory that creates components. Once you have made a component class, then you can use that class to produce as many components as you want.
- Make a new component class by calling `React.createClass();` (Please see below)
- When we make new component classes, we want to store it as a variable so we may use it later
- React Component Classes should always adhere to the UpperCamelCase naming convention
- JSX uses capitalization to distinguish between components and HTML tags. That is why component class names must begin with capital letters! In a JSX element, that capitalized first letter says, "I will be a component instance and not an HTML tag."
#### React.createClass();
React.createClass(); Requirements
1. One argument in the form of a JavaScript Object
	- The JavaScript object argument taken by React.createClass(); acts as a SET OF INSTRUCTIONS explaining how to build the React component
2. Render function whose name is 'render' and whose value is a function()
	- Render function may refer to the entire property, or just the function
3. Return statement (Required by Render function above)
	- USUALLY the return statement returns a JSX expression
- Note ~ Whenever you make a React component, that component inherits all of the properties on the instructions object passed via React.createClass();
- Note ~ ReactDOM.render will tell <MyComponentClass /> to call its render function <= This is why the eslint suggests we keep our class 'stateless' because ReactDOM.render calls the render function for us from a higher level
- Note ~ __NEVER__ declare a variable between React.createClass(); and its render(); function. Rather declare the variable INSIDE of the render(); function
***
#### React.createClass(); render function allowances (before return();)
1. Declare a variable passed from the local file' scope
2. Declare an if/else statement
***
#### Event Handler(s) v Listener(s)
##### Event Handler
- Define event handlers as property values on the instructions object. Like this:
```
React.createClass({
  myFunc: function () {
    alert('Stop it.  Stop hovering.');
  },
  render: function () {
    return (
      <div onHover={this.myFunc}>
      </div>;
    );
  }
});
```
***
Above example passes two properties to React.createClass();
 1. myFunc
 2. Render
- Note ~ When passing a custom function to the render property in createClass, you must call this as the scope is localized
***
#### Component classes can render other component classes
- Render component classes within other component classes by putting a component inside of the render function's return statement.
***
#### Require & module.exports
- Require won't return the entire file containing module.exports. Instead, it will return only the first file's module.exports value.
- module.exports means, "If you require the file that I am in, then here's what you're going to get
***
#### Props ~ How information gets passed from one component to another
- Every component has an object called props
- To see a component's props call this.props
- Any property that gets passed to React.createClass can use this.props
***
#### Props and Attributes
- If you want to pass information to as an attribute that isn't a string, then you have to wrap it in CURLY BRACES like so...
	- <Greeting myInfo={["top", "secret", "lol"]} />
- To display a passed-in prop declare `this.props.name-of-prop` somewhere in that class's render function's RETURN statement.
- The main use of props is to pass information to a component, from a different component
- The component that passes the prop always renders the component that receives the prop.
	- If <X /> is going to be rendered by <Y />, that means that <X /> needs to use module.exports.
- ONLY information declared inside of a return() statement can be seen
- Note ~ props is the name of the object that stores passed-in information. this.props refers to that storage object. At the same time, each piece of passed-in information is also called a prop. props could refer to two pieces of passed-in information, or it could refer to the object that stores those pieces of information
***
#### Pass Event Handler as prop
- We must define an event handler before you can pass an event handler
	- Define an event handler as a property on the instructions object, just like render
- Note ~ Don't forget to declare {this.name-of-prop} when passing a locally declared prop to an attribute
***
#### Naming Convention(s) for Props passed as Event Handler(s)
- Click event ~ handleClick + onClick
- Hover event ~ handleHover + onHover
***
```
handleClick: function () {
  for (var speech = '', i = 0; i < 10000; i++) {
    speech += 'blah ';
  }
  alert(speech);
},
render: function () {
  return <Button onClick={this.handleClick} />;
}
```
- Note ~ Names like onClick only create event listeners if they're used on HTML-like JSX elements. Otherwise, they're just ordinary
***
## Prop children
- Every component's props object has a property named children
- this.props.children will return everything in between a component's opening and closing JSX tags
- If a component has more than one child between its JSX tags, then this.props.children will return those children in an ARRAY
- If a component has only one child, then this.props.children will return the SINGLE CHILD, NOT WRAPPED IN AN ARRAY
***
## Prop default ~ getDefaultProps
- getDefaultProps function should return an object
```
var Example = React.createClass({
  getDefaultProps: function () {
    return { text: 'yo' };
  },
  render: function () {
    return <h1>{this.props.text}</h1>;
  }
});
```
- If no information is passed, the prop will return yo, by default
***
## States
- Flexible = Dynamic
	- Dynamic react components are only supposed to access dynamic information only be accessed in two (2) ways..
		1. this.props
		2. this.state
- Inflexible = Static
***
#### Set Default State ~ getInitialState
- getInitialState always RETURNS AN OBJECT
- State is NEVER PASSED IN FROM THE OUTSIDE
	- A component always sets its own state by calling getInitialState
- getInitialState functions require a return {} function
```
var Example = React.createClass({
  getInitialState: function () {
    return { feeling: 'great' };
  },
  render: function () {
    return <div></div>;
  }
});
```
- Any <Example /> instance will have a state of { feeling: 'great' }
***
#### Access Components State ~ this.state.name-of-property
- Accessing a component's state is just like accessing its props. You use the expression this.state.name-of-property
***
#### Setting State ~ this.setState
- this.setState takes two (2) arguments..
	1. Object 	~ to update component's state
	2. Callback ~ rarely need callback?
- this.setState takes the object that it gets passed, and MERGES the passed object with the component's current state
```
var Example = React.createClass({
  getInitialState: function () {
    return {
      feeling:   'great',
      hungry: false
    };
  },
  render: function () {
    return <div></div>;
  }
});
```
***
Returns...
```
{
  feeling: 'great',
  hungry:  false
}
```
***
If we call...
`this.setState({ hungry: true });`
<Example /> will return a state of...
```
{
  feeling: 'great',
  hungry:  true
}
```
- Note that feeling remains unchanged
	- This is because if there are any properties in the current state that are not part of that object, then those properties remain how they were
## Calling this.setState ~ Event Handler
- We can call this.setState from any property passed to React.createClass BUT...
- We CANNOT call this.setState from the render function
- Any time that we call this.setState, this.setState AUTOMATICALLY calls render as soon as the state has changed
	- this.setState is actually two (2) things
		1. this.setState, immediately followed by.. 
		2. render()
```
getInitialState: function () {
  return { color: green };
},
changeColor: function () {
  var newColor = this.state.color == green ? yellow : green;
  this.setState({ color: newColor });
},
render: function () {
  return (
    <div style={{ background: this.state.color }}>
      <h1>
        Change my color
      </h1>
      <button onClick={this.changeColor}>
        Change color
      </button>
    </div>
  );
}
```
***
## Stateless Component(s)
- Not all React components need to use state
- A component that does not use state is called a STATELESS COMPONENT
- Every React component is either
	1. Stateful
	2. Stateless
- Common to pass information from a STATEFUL component down to a STATELESS component
***
#### Stateless components inheriting from Stateful components
- Rendering an instance is the only way to pass down a prop from a <Parent /> to a <Child />
- <Parent /> must use getInitialState to store whatever property it is going to pass down to <Child />
- <Child /> must use module.exports to inherit from a Stateful component
	- Any component that will be rendered by a different component must use module.exports
- To pass a state <Parent /> must require (import) <Child />
***
## Props v. State
* Props => Store information that can be CHANGED BY A DIFFERENT COMPONENT
* State => Store information that ONLY THE COMPONENT ITSELF CAN CHANGE
***
#### this.setState Stateless Child Component(s) (SCCs) updating Stateful Parent component(s)
Making an SCC
1. Define a state-changing function on the parent
2. Pass state-changing function from Parent must down to Child
  - via return function inside of Parent's render function
3. Define a handleEvent function on the child
```
var ParentClass = React.createClass({
  getInitialState: function () {
    return { totalClicks: 0 };
  },
  handleClick: function () {
    var total = this.state.totalClicks;
    this.setState(
      { totalClicks: total + 1 }
    );
  },
  // The parent passes down
  // handleClick to a child:
  render: function () {
    return (
      <Child onClick={this.handleClick} />
    );
  }
});
```
***
```
var ChildClass = React.createClass({
  render: function () {
    return (
      // The child uses the passed-down
      // handleClick function, accessed
      // here as this.props.onClick,
      // as an event handler:
      <button onClick={this.props.onClick}>
        Click Me!
      </button>
    );
  }
});
```
***
## Child Role(s)
1. Component that **DISPLAYS** the information
2. Component that **CHANGES** the information (Either parent or sibling)
***
### Child, Parent, Sibling state relationship
1. A stateful component defines a function that calls this.setState
2. The stateful component passes that function down to a stateless component
3. The stateless component defines a function that calls this passed-down function, and that can take an event object as an argument
4. The stateless component attaches this new function to an event
5. When an event is detected, the parent's state updates
6. The stateful parent component passes down the state itself, distinct from the ability to change that state, to a different stateless component
7. That stateless component receives the state and displays it
***
## Styling ~ Presentational Component(s) v Container Component(s)
***
### Styling React components..
1. Inline Styling
	- Styles written as attributes inside of DOUBLE curly braces
`<h1 style={{ background: 'lightblue', color: 'darkred' }}>Hello, world</h1>`
2. Define style variable in the top level scope
	- Inside the class file, but outside the React.createClass(); function
```
var styles = { background: 'green', color: 'blue' }
<h1 style={style}>Hello, world</h1>
```
- Note ~ Because we're injecting an object literal, we no longer need the DOUBLE curly braces (as the second curly brace creates an object literal)
3. Define style(s) inside of a separate doc called styles.js
	- Import with styles = require('./styles');
	- Set new variables at the top scope like var divStyle = { background: styles.background, color: styles.color };
***
### Double Curly Brace(s)
- Outermost curly brace(s) are for injecting JavaScript into JSX
  - Declares to JSX everything between us should be read as JavaScript, not JSX
- Innermost curly brace(s) create a JavaScript object literal
	- They are the curly braces that make this a valid object
- The inner curly braces are for creating a __JavaScript object literal__. They are the curly braces that make this a valid object:
***
#### JSX v Web Styles
- JS/CSS => styles are written in hyphenated-lowercase, while in JSX they're written in camelCase
- React  => if you write a style value as a number, then the unit "px" is assumed
***
#### Separating Presentational Component(s) v Display Component(s)
- If a component has to have..
1. State
2. Manipulate props
3. Manage any other complex logic 
- Note ~ If this is the case, then the component SHOULD NOT also have to render HTML-like JSX
***
#### Container(s) Role
- A container should..
	1. Fetch data
	2. Render its CORRESPONDING sub-component
		- Corresponding => a component that shares the same name
```
StockWidgetContainer     => StockWidget
TagCloudContainer        => TagCloud
PartyPooperListContainer => PartyPooperList
```
- In order to..
  - Separate our data-fetching and rendering concerns
  - Make the component recyclable
  - Let PropTypes fail LOUDLY
***
- Presentational layer should.. 
 	- Have one render function
	- No other properties
	- Use module.exports = FileName;
- Presentational components are ALWAYS rendered by container components
	- Any component that gets rendered by a different component should use module.export
***
## Stateless Functional Component(s) (SFC(s))
- If you have a components with NOTHING BUT A RENDER FUNCTION, then instead of using React.createClass, you can write it as JavaScript function
	- Stateless Function Component => Component class written as a function
***
#### Parameter(s)
- Stateless Function Component(s) usually have PROPS passed to them
	- To access props we need to give our SFC a PARAMETER
	- Parameter will equal the props
```
// Normal way to get a prop:
var MyComponentClass = React.createClass({
  render: function () {
    return <h1>{this.props.title}</h1>;
  }
});
// Stateless functional component way to get a prop:
function MyComponentClass (props) {
  return <h1>{props.title}</h1>;
}
// Normal way using a variable:
var MyComponentClass = React.createClass({
  render: function () {
    var title = this.props.title;
    return <h1>{title}</h1>;
  }
});
// Stateless functional component way using a variable:
function MyComponentClass (props) {
  var title = props.title;
  return <h1>{title}</h1>;
}
```
***
## Prop Type(s)
#### Why Prop Types are useful
1. Prop Validation ~ Validation can ensure that your props are doing what they're supposed to be doing. If they are not, then a useful warning will print out in the console
2. Documentation ~ Documenting props makes it easier to glance at a file and quickly understand it. When you have a whole lot of files, and you will, this can be a huge benefit
	- propTypes documents a component class's expected props in an extremely clear way at the top of the createClass function
***
#### Applying Prop Types
- Any time that a component class expects to get passed a prop, that is an opportunity to make a propType
***
#### How to make a Prop Type
1. Create a property on the instructions object called propTypes assigned a value of an object, NOT a function
2. For each prop, add one property to the propTypes object
3. Set the name of each property in propTypes should be the name of an expected prop
	- React.PropTypes.expected-data-type-goes-here
	- For prop `this.props.message` we should have `propTypes: { message: React.PropTypes.string },` above the render function
		- Note ~ React.PropTypes is capitalized, unlike propTypes
***
## .isRequired
- If you add .isRequired to a propType, then you will get a console warning if that prop isn't sent
***
#### Stateless Functional Components with PropTypes
- We add our PropTypes to instructions objects BUT STMs don't have instruction objects..
- To write propTypes for an STM, you define a propTypes object, as a PROPERTY OF THE STATELESS FUNCTIONAL COMPONENT ITSELF
```
function Example (props) {
  return <h1>{props.message}</h1>;
}
Example.propTypes = {
  message: React.PropTypes.string.isRequired
};
```
***
## React Forms (Input field(s))
- Best way of listening for to changes inside of the input field is by passing an input field an onChange attribute and setting up an event handler to listen for the onChange event(s)
- When a user types or deletes in the <input /> field, then that will trigger a change event, which will call handleUserInput
	- handleUserInput will set this.state.userInput equal to whatever text is currently in the input field
```
var Input = React.createClass({
  getInitialState: function () {
    return { userInput: '' }
  },
  handleUserInput: function (e) {
    this.setState({
      userInput: e.target.value
    });
  },
  render: function () {
    return (
      <div>
        <input 
          type="text"
          value={this.state.userInput}
          onChange={this.handleUserInput} />
        <h1>{this.state.userInput}</h1>
      </div>
    );
  }
});
```
***
## Controlled Component v Uncontrolled Component
- Uncontrolled Component ~ Maintains its own internal state 
- Controlled Component 	 ~ Does not maintain any internal state
  - Information must be controlled by someone else
  - Has no memory. If you ask it for information about itself, then it will have to get that information through props
  - Most React components are Controlled
***
## LifeCycle Methods
- METHODS ON THE OBJECT passed to React.createClass, getInitialState, and getDefaultProps
- LifeCycle Methods are AUTOMATICALLY called at certain moments in a component's life
- Could write so the lifecycle method is called..
	1. BEFORE a component renders for the first time
	2. AFTER a component renders, every time EXCEPT FOR THE FIRST TIME
***
### LifeCycle Method Categories
1. Mounting ~ Process of component instance attaching to the DOM
2. Updating
3. UnMounting
***
### 1 ~ Mounting LifeCycle Method(s)
- Mounting lifecycle methods get called/"mounts" WHEN IT RENDERS FOR THE FIRST TIME
- Mounting lifecycle methods..
	a. componentWillMount
	b. render
	c. componentDidMount
	-  When a component mounts, it AUTOMATICALLY CALLS THESE THREE METHODS, IN ORDER
***
#### 1a ~ componentWillMount
- componentWillMount called when component RENDERS FOR THE FIRST TIME
- componentWillMount called BEFORE THE RENDERING BEGINS
- componentWillMount usually used if you need to do something only the first time that a component renders
- Note ~ We can call this.setState from within componentWillMount
***
#### 1b ~ render
- Note ~ render belongs to two categories..
	1. mounting lifecycle methods 
	2. updating lifecycle methods
***
#### 1c ~ componentDidMount
- For when we want something to happen AFTER THE FIRST RENDERING after the HTML has finished loading
- AJAX calls are made
- Connect to external libraries and/or APIs
- Set timers using setTimeout or setInterval
***
## Updating and Unmounting LifeCycle methods
### Updating
- A component updates every time that it renders, STARTING WITH THE SECOND RENDER
- Note ~ The first time that a component instance renders, IT DOES NOT UPDATE
***
### Five (5) Updating LifeCycle Methods
1. componentWillReceiveProps 
	- Called BEFORE the rendering begins
	- Only called if the component will receive props
	- componentWillReceiveProps automatically gets passed one argument: an object called nextProps
		- nextProps is a preview of the upcoming props object that the component is about to receive
	- Useful for comparing incoming props to current props or state, and deciding what to render based on that comparison
2. shouldComponentUpdate
	- Gets called AFTER componentWillReceiveProps, but still BEFORE the rendering begins
	- Best way to use shouldComponentUpdate is to have it return false only under certain conditions. If those conditions are met, then your component will not update
		- Stops all LifeCycle Methods. Not even render is called
	- Automatically receives two (2) arguments: 
		1. nextProps
		2. nextState
		- Normal to compare nextProps and nextState to the current this.props and this.state
3. componentWillUpdate
	- Called in between shouldComponentUpdate and render
	- Automatically receives two (2) arguments:
		1. nextProps
		2. nextState
	- Note ~ You cannot call this.setState from the body of componentWillUpdate
	- Primary purpose of componentWillUpdate is to interact with things OUTSIDE OF THE REACT ARCHITECTURE
		- Such as checking the window size or interacting with an API
4. render
5. componentDidUpdate
	- componentDidUpdate gets called AFTER any rendered HTML has finished loading
	- Automatically passed two (2) arguments: 
		1. prevProps ~ props BEFORE the current updating period began
		2. prevState ~ state BEFORE the current updating period began 
		- May then compare them to the CURRENT props and state
	- Useful for interacting with things OUTSIDE of the React environment, like the browser or APIs
		- Similar to componentWillUpdate, except it happens after render, instead of before
***
## Unmounting
- Occurs when the component is REMOVED from the DOM
	- Could happen if..
		a. The DOM is re-rendered WITHOUT THE COMPONENT
		b. If the user navigates to a different website
		c. User closes their web browser
***
### componentWillUnmount
- ONLY unmounting LifeCycle method
- Called RIGHT BEFORE a component is removed from the DOM
- Useful for component(s) which initiates any methods that require cleanup
- When a component instance updates it AUTOMATICALLY calls the five (5) updating lifecycle methods, in order
