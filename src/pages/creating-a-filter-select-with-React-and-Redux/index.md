---
title: Creating a Filter Select with React + Redux
date: "2018-11-19T00:01"
---

## React + Redux Typeahead with config.

We are going to be building a Typeahead Select Field using React with Redux. Let’s get started.

To get started make sure you have ‘create-react-app’ installed. If not, go here to install it. It’s cool, I’ll wait…

Once you have that installed. Let’s go to step 1.

### Step 1:  Generate a new React app.

In your terminal type:

` create-react-app typeahead`

Wait for the necessary files to download and install in your project folder.

### Step 2:  Let’s Build the UI first.

Once your React app has been generated open up `./src/App.js`
_This is the default component that ‘create-react-app’ gives you._

Install Bootstrap using your terminal.
`npm install —save bootstrap`

Go ahead and clear out everything in between the `<div className=“App”>…</div>` tags so it looks like this:

.src/**app.js**
```
import React, { Component } from 'react';
import logo from './logo.svg';
import './App.css';

class App extends Component {
  render() {
    return (
      <div className="App">

      </div>
    );
  }
}

export default App;
```

Now, let’s create a new folder called **components** in the **src** directory of our project. In there we are going to create our first UI component.  Create a new file called ‘SelectComponent.’  This file (for now) will contain the basic HTML setup for our custom Select component.

.src/components/**SelectComponent.js**
```
import React from 'react'

export default() =>
<div>
	<input
		type="text"
		placeholder="search me..."
</div>
```

Right now, there’s nothing special about our component. It’s a simple component that renders out an input field.  Next, we’ll put some basic styles.  Add the following to the SelectComponent:

.src/components/**SelectComponent.js**
```
import React from 'react'

const styles = {
	searchField: {
		fontSize: '14px',
	},
}

export default()=> /.... etc.../

```

To add the new style to our field, simply add the ‘style’ property to the input field like so:

.scr/components/**SelectComponent.js**
```
export default () =>
<div>
	<input
 		type="text"
  		placeholder="search me..."
  		*style={styles.searchField}*
    />
</div>
```

We are going to need a place to show the selections made (pills), the search field, and the options list.  

Let’s modify .src/**index.js** by importing bootstrap.
```
import React from 'react'
import ReactDOM from 'react-dom'
*import 'bootstrap/dist/css/bootstrap.css'*
import './index.css'
import App from './App'
import registerServiceWorker from './registerServiceWorker'
//import 'bootstrap/dist/css/bootstrap-theme.css'

ReactDOM.render(
  <App />,
  document.getElementById('root')
);

registerServiceWorker();
```

We will also fix up our SelectComponent a little bit, and add some styles and some place holders so we have the basic UI ready.

.src/components/**SelectComponent.js**
```
import React from 'react'

const styles = {
  searchField: {
    border: 'none',
    paddingLeft: '5px'
  },
  filterSelect: {
    clear: 'both',
    display: 'block',
    float: 'left',
    border: '1px solid #ccc',
    borderRadius: '3px',
    minHeight: '40px',
    height: 'auto',
    lineHeight: '38px',
    listStyle: 'none',
    marginBottom: '0px',
    paddingLeft: '2px',
    width: '100%',
  },
  selectedBadge: {
	    listStyle: 'none',
	},
  lastChild: {
    width: 'initial',
  },
  selectorBox: {
    listStyle: 'none',
  },
  optionItem: {
    width: '120px'
  }
}

export default () =>
<div className="row">
  <div className="col-xs-12 col-md-12">

    <ul style={styles.filterSelect}>

      <li className="p-0 mr-1 float-left" style={styles.selectedBadge}>
        <button className="btn btn-primary btn-sm" type="button">
          Selection 1
          <span className="badge">x</span>
        </button>
      </li>

      <li style={styles.lastChild}>
        <input
          type="text"
          placeholder="search me..."
          className="float-left"
          style={styles.searchField}
        />
      </li>

    </ul>

  </div>

  <div className="col-xs-12 col-md-12">
    {/*
      This list will hold our options.
    */}
    <ul className="list-group ">
      <button type="button" className="list-group-item list-group-item-action">Option 1</button>
      <button type="button" className="list-group-item list-group-item-action">Option 2</button>
      <button type="button" className="list-group-item list-group-item-action">Option 3</button>
    </ul>
  </div>
</div>

```

## Connecting the UI to Redux
What will our component need from the state. The state is essentially the index reducer file that will contain all of our state properties. Let me demonstrate what I mean.

1. Create a new folder in the `./src`  directory called `reducers`
2. Create a file called ‘index.js’ in the new directory you just created.

.src/reducers/**index.js**
```
import { combineReducers } from 'redux'

const selectorBoxApp = combineReducers({
  config: {},
  options: [],
  selections: [],
})

export default selectorBoxApp
```

Our reducer will have the following sub state properties:

* config: (_object_)  will hold our settings for making this either single or multi-select.
* options: (_array of objects_) These will be the options that are available.
* selections: (_array_) This is where we are going to hold the selections the user makes.

Create the the following files:
* ./src/reducers/config.js
* ./src/reducers/options.js
* ./src/reducers/selections.js

Open ./src/reducers/**config.js**
```

{/* Default state. */}
const defaultConfigState = {
  searchText: '',
  onSelectionMade: 'reduced',
  onType: 'reduced',
}

const config = ( state = defaultConfigState , action ) => {

  switch (action.type) {

    default:
      return state

  }

}

export default config
```

Now we have to add it to our `./src/reduces/index.js` file.
```
line 2: import config from './config'

...

line 4:
const selectorBoxApp = combineReducers({
  *config: config*,
  options: [],
  selections: [],
})

...
```

Let’s do the same with the other two state properties options, and selections.

Open ./src/reducers/**options.js**
```
const defaultOptionsState = {
  field1: [{
    value: 1,
    text: 'One',
  }]
}

const options = ( state = defaultOptionsState, action ) => {

  switch (action.type) {

    default:
      return state

  }

}

export default options

```

Open ./src/reducers/**selections.js**
```
const defaultSelectionsState = {
    field1: []
}

const selections = ( state = defaultSelectionsState, action ) => {

  switch (action.type) {

    default:
      return state

  }

}

export default selections
```


Okay now update the index reducer file by adding the two reducers you just created.

./src/reducers/**index.js**
```
import { combineReducers } from 'redux'
import config from './config'
*import options from './options'*
*import selections from './selections'*

const selectorBoxApp = combineReducers({
  config: config,
  *options: options,*
  *selections: selections,*
})

export default selectorBoxApp

```

Okay, now we are ready to rock n’ roll. And if you’re doubting me, and my rock n’ roll abilities most of my friends will let you know how I like to shred on the guitar. Any who, back to business.

### Connecting Redux.

So now that we have our components and reducers in place, we are going to connect Redux to our app, so that we can use the files we’ve just created.

### Actions

What is our app going to do to let redux know that something happened?

Go ahead and create a new directory called ‘actions’ in  ‘./src’

In there we will add our action types (fancy word for an event name constant) and our action creators fancy term for the functions that dispatch said action types so that our app knows something has happened.

[Explain why]

## Containers
[Connecting the UI (components) using containers]
