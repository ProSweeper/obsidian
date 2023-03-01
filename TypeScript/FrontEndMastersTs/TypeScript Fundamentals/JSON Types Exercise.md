Define a type that describes any allowable JSON value

Here are the requirements: ![[Pasted image 20230301130607.png]]

Here is the starting point: ![[Pasted image 20230301130748.png]]

```typescript
type Primitive = string | number | boolean | null
// define the primitive types since they are all allowed

type JSONObject = {[k: string]: JSONValue}
// use index signatures to define the object 
type JSONArray = JSONValue[]
// both JSONArray and JSONObject are leveraging recursive types
type JSONValue = Primitive | JSONObject | JSONArray
```