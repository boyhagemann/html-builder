# html-builder
A proof-of-concept setup for build SPA pages with a JSON config file

#### Implementations
* [Cycle.js engine] (http://github.com/boyhagemann/html-builder-engine-cycle) (work in progress, see [Roadmap] (/README.md#roadmap))
* React.js engine (coming up)
* Elm engine (coming up)

#### Guidelines
* The JSON config should be the glue between a back-end and front-end implementation.
* All information for a full-blown html page should be in the JSON config.
* The JSON must follow the [HTML Builder Protocol] (/protocol.md) and eventual a defined schema.
* Here are some [examples of config files] (/examples) in different formats.

# Architecture
The system consists of several concepts that communicate in some way to each other.

##### Producers
A producer is a stream of data which values change over time.
They are actually the triggers for sending a message thru Actions.
The JSON config holds a list of producers used in the application.
Examples of producers are:
* [client] user events (click, change, keyUp, drag, resize)
* [server] timers (they can produce values over time, which is set in a store)
* [server] sockets (the push values from external sources)
* [server] stores (when values change in the store, an Action message is sent)

##### Actions
Actions are only messages that accept a certain payload of metadata.
These Actions or part of things called Reducers, but thats only for keeping code managable.
Actions are not part of the JSON config, but only exist in the application code.
When an action is called, that could result in 2 things:
* calling more Actions
* doing a side effect (data fetching, I/O operations, render DOM)

##### Stores
The are the holders of the state.
Any dataset that has more or less influence on the application should be in here.
Stores can only be manipulated thru Actions.
The JSON config holds a list of stores used in the application.

Examples of stores are:
* Data collected from a resource
* Window information (for the current url and the screen size for example)
* Validation errors (for displaying form messages)
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
This gives us the abilitiy to do "time travel debugging", share states, unlimited undos of every action, etc.

> Every store can have an (intitial) set of data included in the JSON config.
> Actually, all json config keys are then all stores automatically!

##### Side effects
An action can trigger a side effect, such as:
* render the DOM based on an Action payload (when a store changes, an Action message was sent).
* call a REST resource with a Promise (a user clicked a button that sends an Action message with the resource info as payload).
* update a store with data from an Action payload (the Promise sends the Action message with the response as payload). 

##### Conditions
Components can be rendered only when some conditions are met.
Examples of conditional rendering are:
* Show/hide a button
* Enable/disable button
* Show page when url matches
* Media queries on screen size change
* Show/hide on error

> A cool thing about Conditions is that they can be used to matched to a url.
> This means we can also nest Conditions within Conditions to get subpages and have breadcrumbs.

# Operators
On top of the producers there can be many Operators.
An operator can alter the value or time of the Producer.
This can be very helpful to have control over the data thats comming from the Producer.
Examples of operators are (grouped by type):

#### Action
> Format: action(param)
* debounce(time)
* delay(time)
* retry(times)
* defaults(key, value)
* payload(object)

#### Validation
> Format: render(field, [message, [condition]])
* string
* email
* required

#### Collection
> Format: collection(params)
* limit(count)
* skip(count)
* pluck(key)
* parents(node) -> for breadcrumbs

#### Modifier
> Format: modify(field, [params, [condition]])
* trim(strings)
* length(size) -> elipsis ...
* slug(field)
* upper(field)
* translate(text, lang)
* price(field, locale)

#### Mapping
> Format: map(field, alias, [condition])
* move
* clone
* replace

#### Render
> Format: render(condition)
* show
* hide
* active (for events)

##### Style
> Format: style(property, value, [condition])
* style
* class

#### Debug
> Format: debug(message, [condition])
* log

> Operators can work on both the properties and the content of a component.

# Components
All components can nest multiple child components

#### Node
They simple render to html with some given properties.
The properties can be provided by other components or by passing it thru a configuration form.

#### Event
An event is always bound to a rendering node.
They are components and not operators.
This is because the Events can have operators on them, so the need to be components.
Because they relate to one node, it should be a separate list in the CMS.

#### Collection
This component can take a collection of data from the store and map each item to the child components.
For instance, we can have a Collection that points to a collection of products.
Within this Collection component, we can have 2 child components: 
* a Heading component with the product title
* a Text component with the product description

#### Item
The same as a Collection component, but only for single item stores.

#### Form
A Form component is a wrapper for various input components.
With this wrapper we can validate or send data in one go.
This can be handy for having a Progress component, in which a user sees his progress in steps.
> A form must scan all children components somehow for 'validate' operators.
> It must keep a dynamic store of these fields and the status of validation.

#### Partial
This component is a reference to another node in the structure. 
It can be any node, either the current tree or another one.
It will render exactly the same as the original.
If the original changes, then these changes are also visible in this Partial component.
A partial can have multiple Section components.

#### Section
A Section component is an empty placeholder that can only be used in a Partial component.
It allows child components to be nested inside a Partial component.
> A section can have child components just like a regular component.
> These components will be overridden once partial is filled with other components in that section.

## Component specific operators
Each component can have its own operators.
They provide finegrained control over its data or rendering.
Here are some examples for each component.

| Component         | Operator type                                     |
|-------------------|---------------------------------------------------|
| Node              | Debug, Rendering                                  |
| Event             | Debug, Rendering, Action                          |
| Collection        | Debug, Validation, Modifier, Mapping, Collection  |
| Item              | Debug, Validation, Modifier, Mapping              |
| Form              | Validation                                        |
| Partial           | Mapping                                           |
| Section           | Mapping                                           |


# Booting
Assume we have a the most barebone situation for the config.
What is the minimal step to do something?
And what do we want to do?
For now, let's assume we want to render html.
There should be an "init" or "boot" key in the json config.
That should hold a collection of initial action messages that need to be send.
Here we can define that we need to fetch nodes first and put them in the store.
When the store updates, then the nodes get automatically rendered thanks to the driver/reducer/side effect.

# Roadmap
* [x] Build html from the JSON
* [x] Bind events to html identifiers
* [x] Reuse components with own event scope
* [x] Fetch async data
* [x] Add url history driver
* [ ] Use the collection component
* [ ] Use the item component
* [ ] Use the form component
* [ ] Use the partial component
* [ ] Use the section component
* [ ] Implement operators


# Questions that need a solution
* How to handle translations?
* Is every input component explicitly bound to a store?
* Is rendering a component tree actually an Action message sending multiple Action messages for the node children?
* Maybe introduce a 'private' key in the config that holds sensitive information?
* We could place node partials as a store in the config? So nodes can point to other nodes, a.k.a. relations.
* Introduce a "yield" operator. Every component inside a partial can then be assigned to a section.
* Is an operator an Action?

# Ideas for the cms part
* Use autocomplete search to find nodes based on their type and contents
* Tag nodes to quickly get a list view of all the tagged nodes
* Quick controls for changing the behaviour directly from a node list without a config page
* Assign form controls for saved partials, so a partial has its own reusable configuration form
* Import from urls and transform them into nodes.
* If a partial has multiple sections, show different tabs for their content.