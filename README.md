# html-builder
A proof-of-concept setup for build SPA pages with a JSON config file

### Guidelines
* The JSON config should be the glue between a back-end and front-end implementation.
* All information for a full-blown html page should be in the JSON config.

## Config structure
* nodes (the html tree structure)
* events (the action triggers)

### Standards
There should be standards for declaring nodes in the JSON config.
Same goes for the event declaration

### Config building
A (third party) application should be responsible for building the JSON config.
This config can then either be stored in a central storage or in a domain-specific storage.

### Config parsing
There should be a package that takes the JSON config an can turn it into a javascript SPA application.
Candidates for parsing the JSON config into javascript are:
* Cycle.js
* React.js
* Elm

# Data flow

## DOM
The html DOM can be rendered with the JSON config.

## Events
The events are bound to specific elements in the DOM.
These events trigger Actions with a certain payload.
Examples of events are:
* button clicks
* window resize
* url change

## Actions
When an action is called, that could result in 2 things:
* calling more Actions
* doing a side effect (data fetching, I/O operations)
Every Action can update the current state.

## Stores
The state is being stored in Stores.
The can only be manipulated be thru Actions.
Examples of stores are:
* rest resources
* local storage
* window (screen width, url)

## Conditional rendering
Components can be rendered only when some conditions are met.
Examples of conditional rendering are:
* Show/hide a button
* Enable/disable button
* Show page when url matches
* Media queries on screen size change

### Roadmap
* [x] Build html from the JSON
* [x] Bind events to html identifiers
* [ ] Reuse components with own event scope
* [ ] Fetch async data
* [ ] Use conditions as components

