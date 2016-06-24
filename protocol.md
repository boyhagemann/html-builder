
# HTML Builder Protocol
This should contain all the specifications needed to build an application with the Html Builder.


## Ideas
There should be an init list of actions
```yaml
init:
  - action: FETCH
    payload:
      store: nodes
stores:
  nodes:
    connection:
      adapter: rest
      config:
        uri: http://domain.com/rest/nodes
        method: GET
    driver: dom-renderer
    data:
      - ... some initial node data
```


## 1. Root
#### 1.1. The root of the file must contain three keys in arbitrary order:
* nodes
* producers
* stores
```
{
  "nodes": ...,
  "stores": ...,
  "producers": ...,
}
```

## 2. Nodes
#### 2.1. The root `nodes` key can only be a collection.

```
...
  "nodes": [],
...
```
#### 2.2. A node can accept these keys:
* type
* properties
* children
