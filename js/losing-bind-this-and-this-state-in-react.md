# Getting rid of .bind(this) and this.state = in React component

If you use React, you probably have to write some of '.bind(this)' code and 'this.state =' in constructor
Cons:
- it looks pretty ugly
- it’s taking up some extra space in the codebase

# Reference
 
[Class Fields Declaration](https://github.com/tc39/proposal-class-fields) proposal is currently at Stage 3 in the [TC-39](https://github.com/tc39/proposals) process

### Bad

React components often have methods tied to DOM events. To ensure this resolves to the component, it’s necessary to bind every method accordingly in the constructor.
Also there you usually set initial state in the constructor of the class.

````javascript
  import React, { Component } from "react";
  
  class ButtonComponent extends Component {
    constructor(props) {
      super(props);
  
      this.state = { toggle: false };
  
      this.toggleButtonTitle = this.toggleButtonTitle.bind(this);
    }
  
    toggleButtonTitle() {
      this.setState(prevState => ({ toggle: !prevState.toggle }));
    }
  
    render() {
      const classes = this.state.toggle ? 'show' : 'hide';
  
      return (
        <div>
          <button onClick={this.toggleButtonTitle} className={classes}>
            'Save'
          </button>
        </div>
      );
    }
  }
  
  export default ButtonComponent;
````

### Good

If toggleButtonTitle is defined as a class field, it can be set to a function using an ES6 => fat arrow, which automatically binds it to the defining object.
The state can also be declared as a class field so no constructor is required.

````javascript
  import React, { Component } from 'react';
  
  class ButtonComponent extends Component {
    state = { toggle: false };
  
    toggleButtonTitle = () => {
      this.setState(prevState => ({ toggle: !prevState.toggle }));
    };
  
    render() {
      const classes = this.state.toggle ? 'show' : 'hide';
  
      return (
        <div>
          <button onClick={this.toggleButtonTitle} className={classes}>
            'Save'
          </button>
        </div>
      );
    }
  }
  
  export default ButtonComponent;
````
