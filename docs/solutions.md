# Solutions
These are general ideas how to solve specific cases.

## Conditions
We can use conditions as regular components.
This way we can nest very complex conditions right in the view.
If we have conditions that are used regurarly thru the application, we can reuse them with partials.
This makes the interface very consistent.

## Booting
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