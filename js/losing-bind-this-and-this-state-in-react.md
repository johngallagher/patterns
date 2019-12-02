# Getting rid of .bind(this) and this.state = in React component

If you use React, you probably have to write some of '.bind(this)' code and 'this.state =' in constructor
Cons:
- it looks pretty ugly
- it’s taking up some extra space in the codebase

### Bad

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
