# html-builder
A proof-of-concept setup for build SPA pages with a JSON config file

#### Implementations
* [Cycle.js engine] (http://github.com/boyhagemann/html-builder-engine-cycle) (work in progress)
* React.js engine (coming up)
* Elm engine (coming up)

#### Guidelines
* The JSON config should be the glue between a back-end and front-end implementation.
* All information for a full-blown html page should be in the JSON config.
* The JSON must follow the [HTML Builder Protocol] (/protocol.md).

# Architecture
The system consists of several concepts that communicate in some way to each other.

#### Producers
A producer is a stream of data which values change over time.
They are actually the triggers for sending a message thru Actions.
The JSON config holds a list of producers used in the application.
Examples of producers are:
* user events (click, change, keyUp, drag, resize)
* timers (they can produce values over time)
* sockets (the push values from external sources)
* stores (when values change in the store, an Action message is sent)

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
* The initial JSON config to build the entire system!

Every store can have a driver (reducer):
* REST
* SOAP
* DB
* Local storage

All data is being stored in a plain object, used by the application.
This gives us the abilitiy to do "time travel debugging", share states, unlimited undos of every action, etc.

#### Side effects
An action can trigger a side effect, such as:
* render the DOM based on an Action payload (when a store changes, an Action message was sent).
* call a REST resource with a Promise (a user clicked a button that sends an Action message with the resource info as payload).
* update a store with data from an Action payload (the Promise sends the Action message with the response as payload). 

#### Operators
On top of the producers there can be many Operators.
An operator can alter the value or time of the Producer.
This can be very helpful to have control over the data thats comming from the Producer.
Examples for operators are:
* debounce(time)
* delay(time)
* limit(count)
* skip(count)
* retry(times)
* pluck(key)
* log(value => message)

# Components
All components can nest multiple child components

## Dumb components
These components have just a simple task.
You give them some properties, and they will render html.

## Smart components 
These type of components don't actually render anything.
Instead, they do some other things that can be pretty useful.
For now, the most needed components were actually the ones that inspired by template engines.
This makes sense, because we are doing the same thing, only now with a central configration format.

#### Condition
Components can be rendered only when some conditions are met.
Examples of conditional rendering are:
* Show/hide a button
* Enable/disable button
* Show page when url matches
* Media queries on screen size change

#### Collection
This component can take a collection of data from the store and map each item to the child components.
For instance, we can have a Collection that points to a collection of products.
Within this Collection component, we can have 2 child components: 
* a Heading component with the product title
* a Text component with the product description

#### Partial
This component is a reference to another node in the structure. 
It can be any node, either the current tree or another one.
It will render exactly the same as the original.
If the original changes, then these changes are also visible in this Partial component.
A partial can have multiple Section components.

#### Section
A Section component is an empty placeholder that can only be used in a Partial component.
It allows child components to be nested inside a Partial component.

# Roadmap
* [x] Build html from the JSON
* [x] Bind events to html identifiers
* [x] Reuse components with own event scope
* [x] Fetch async data
* [ ] Use the condition component
* [ ] Use the collection component
* [ ] Use the partial component
* [ ] Use the section component


# Questions that need a solution
* How to handle translations?
* Do we need a Try and Catch component?
* Is every input component explicitly bound to a store?
* Is rendering a component tree actually an Action message sending multiple Action messages for the node children?
* Maybe introduce a 'private' key in the config that holds sensitive information?

# Ideas for the cms part
* Use autocomplete search to find nodes based on their type and contents
* Tag nodes to quickly get a list view of all the tagged nodes

