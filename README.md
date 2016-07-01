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

#### Read more
* [Architecture] (/docs/architecture.md)
* [Components] (/docs/components.md)
* [Operators] (/docs/operators.md)


# Booting
Assume we have a the most barebone situation for the config.
What is the minimal step to do something?
And what do we want to do?
For now, let's assume we want to render html.
There should be an "init" or "boot" key in the json config.
That should hold a collection of initial action messages that need to be send.
Here we can define that we need to fetch nodes first and put them in the store.
When the store updates, then the nodes get automatically rendered thanks to the driver/reducer/side effect.

## Input
Input components are a special case.
Their state needs to be stored on change.
Preferably, we don't want any logic hardcoded in the compiler.
So this logic should be in the JSON config itself.
That can be easily done with these steps
* Add an event with type "change" for this node.
* Optionally, this event can have a "debounce" or "throttle" operator to restrain the stream of data.
* On the node, add a "map" operator and bind it to the "localstorage" store with "the.input.name" as key.
* Also add a "defaults" operator with value "{the.input.name}".

# Roadmap
* [x] Build html from the JSON
* [x] Bind events to html identifiers
* [x] Reuse components with own event scope
* [x] Fetch async data
* [x] Add url history driver
* [ ] Use the event component
* [ ] Use the collection component
* [ ] Use the item component
* [ ] Use the form component
* [ ] Use the partial component
* [ ] Use the section component
* [ ] Implement operators


# Questions that need a solution
* How to handle translations?
* How to handle files?
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