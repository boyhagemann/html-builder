# Operators
On top of the producers there can be many Operators.
An operator can alter the value or time of the Producer.
This can be very helpful to have control over the data thats comming from the Producer.

## Examples

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

#### Style
> Format: style(property, value, [condition])
* style
* class

#### Debug
> Format: debug(message, [condition])
* log

> Operators can work on both the properties and the content of a component.~~~~~~~~