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

