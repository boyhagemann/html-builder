
# Architecture
The system consists of several concepts that communicate in some way to each other.

#### Events
A producer is a stream of data which values change over time.
They are actually the triggers for sending a message thru Actions.
The JSON config holds a list of events used in the application.

#### Actions
Actions are only messages that accept a certain payload of metadata.
These Actions or part of things called Reducers, but thats only for keeping code managable.
Actions are not part of the JSON config, but only exist in the application code.
When an action is called, that could result in 2 things:
* calling more Actions
* doing a side effect (data fetching, I/O operations, render DOM)

#### Stores
The are the holders of the state.
Any dataset that has more or less influence on the application should be in here.
Stores can only be manipulated thru Actions.
The JSON config holds a list of stores used in the application.

Examples of stores are:
* Data collected from a resource
* Window information (for the current url and the screen size for example)
* Validation errors (for displaying form messages)
* Current input values for building requests
* Translations
* A simple counter
* The initial JSON config to build the entire system!

Every store can have a driver (reducer):
* REST
* SOAP
* DB
* Local storage
* Renderer (for the nodes)

All data is being stored in a plain object, used by the application.
This gives us the ability to do "time travel debugging", share states, unlimited undos of every action, etc.

> Every store can have an (initial) set of data included in the JSON config.
> Actually, all json config keys are then all stores automatically!

#### Side effects
An action can trigger a side effect, such as:
* render the DOM based on an Action payload (when a store changes, an Action message was sent).
* call a REST resource with a Promise (a user clicked a button that sends an Action message with the resource info as payload).
* update a store with data from an Action payload (the Promise sends the Action message with the response as payload). 

#### Conditions
Components can be rendered only when some conditions are met.
Examples of conditional rendering are:
* Show/hide a button
* Enable/disable button
* Show page when url matches
* Media queries on screen size change
* Show/hide on error

> A cool thing about Conditions is that they can be used to matched to a url.
> This means we can also nest Conditions within Conditions to get subpages and have breadcrumbs.