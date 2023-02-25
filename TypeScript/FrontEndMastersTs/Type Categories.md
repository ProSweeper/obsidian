## Typing Functions
- defining a return type for a function makes it so any errors that the function may have are brought up when the function is declared
- this helps reduce noise in the program
```typescript
function add(a:number, b:number): number {
	return a + b;
}
```
- the above function requires that a number is returned, any other data type will throw an error
- explicit return types makes sure you state your intentions upfront and surfaces problems where you need to fix them

## Objects
- in general object types are defined by:
	- names of the properties
	- types of these properties
- if we had a concept of a `car` like a "2002 Toyota Corolla" with properties:
	- `make` : the manufacturer (Toyota)
	- `model` the particular product (corolla)
	- `year` the model year (2002)
- The type that would represent this would be: 
```TypeScript
{
make: string
model: string
year: number
}
```
- we can use this type with a variable using the following notation
```typescript
let car: {
	make: string
	model: string
	year: number
}
```
- and now when we update the car variable, its properties must follow the typing we have defined

### Optional Properties
- we can take our car example a little further by adding a fourth property that is only present sometimes
| Property name   | Is Present | Type     | Note |
| --------------- | ---------- | -------- | ---- |
| `make`          | Always     | `string` |      |
| `model`         | Always     | `string` |      |
| `year`          | Always     | `number` |      |
| `chargeVoltage` | Sometimes  | `number` | not present unless car is electric
- We can state the property is optional using the `?` operator
```typescript
let car: {
	make: string
	model: string
	year: number
	chargeVoltage?: number
}
```
- and now we have the option to have an electric car
- When dealing with optional properties it is common practice to use type guards to allow the program to run without error
```typescript
function printCar(car: {
	make: string
	model: string
	year: number
	chargeVoltage?: number
}) {
	let str = `${car.make} ${car.model} ${car.year}`
	car.chargeVoltage // will either be a number or undefined
	// 
	if (typeof car.chargeVoltage !== "undefined") {
		str += `${car.chargeVoltage}`
	}
	console.log(str)
}
```
- since there is no code that guarantees the `chargeVoltage` will be there we need to include the type guard or else the code will throw an error
   
### Excess Property Checking
- TypeScript will not allow properties that were not included in the declaration of the type to be included in any other instances 
```typescript
function printCar(car: {
	make: string
	model: string
	year: number
	chargeVoltage?: number
}) {
	let str = `${car.make} ${car.model} ${car.year}`
	car.chargeVoltage // will either be a number or undefined
	// 
	if (typeof car.chargeVoltage !== "undefined") {
		str += `${car.chargeVoltage}`
	}
	console.log(str)
}

const myCar = {
	make: "dodge",
	model: "charger",
	year: 2020,
	color: "red"
}
printCar(myCar) // this will throw an error since color was not included in the car type when it was declared
```

## Index Signatures
- sometimes we need to represent a type for dictionaries, where values of a consistent type are retrievable by keys
- lets look at this collection of phone numbers 
```typescript
const phones = {
	home: {country: "+1", area: "211", number: "653-4522"},
	work: {country: "+1", area: "224", number: "123-4252"},
	fax: {country: "+1", area: "431", number: "624-5234"},
}
```
- it seems we can store numbers under a "key", in this case `home` `work` or `fax` and each phone number is comprised of three strings
- we could describe this value using what is known as an index signature
```typescript
const phones: {
	[k: string]: {
		country: string
		area: string
		number: string
	} | undefined
} = {}
```
- note the box notation for accessing the key from an object/dictionary
- we put the undefined option just in case we initialize an empty object

## Array Types
- to add a type to an array all we have to do is add a pair of square brackets to the end of any member  
```typescript
const myArr : string[] = [];
```
- You can also have more complicated array types, we could have an array only for storing our cars from above

## Tuples
- sometimes we want to work with a multi element, ordered data structure, where each position of each item has special meaning. This can be done using a tuple
```typescript
//           Year,  Make,     Model
let myCar = [2002, "Toyota", "Corolla"]
const [year, make, model] = myCar
```
- This makes it really nice for destructured assignment
- This does however leave room for issues with typescript casting inferred types ![[Pasted image 20230225104724.png]]
- As seen above typescript assumes that anything in the array can be a string or a number, not exactly the behavior we want in this instance
- This means we must explicitly state the type of  a tuple when we declare one
```typescript
let myCar: [number, string, string] = [
	2002,
	"Toyota",
	"Corolla",
]
```
- this will lead to the following when trying to assign outside of the boundaries we set ![[Pasted image 20230225105810.png]]

### Limitations
- As of TypeScript 4.3 there is limited support for enforcing tuple length
- You do get support on assignment ![[Pasted image 20230225110120.png]]
- But you do not get support around `push()` and `pop()` 
```typescript
const numPair: [number, number] = [4,5];
numPair.push(6) // [4,5,6]
numPair.pop() // [4, 5]
numPair.pop() // [4]
numPair.pop() // []
```

## Structural vs Nominal Types

### What is Type Checking
- can be thought of as a task to evaluate whether a value is compatible with what it is being asked to do 
- In the function bellow typescript will check to make sure the value `myValue` is compatible with what the function `foo()` is expecting to receive 
```typescript
function foo(x) {
//	... mystery code ...
}
// Type Checking 
// is myVal type-equivelent to what 'foo' wants to receive
foo(myVal)
```
- This question of type checking can occur when a function is being called or when a variable is being assigned 
- In the case of variable assigning we as is the value  `y`  holds compatible with what `x` allows when we try to assign  `x = y`  
- We can have type checks for return types in a function 
```typescript
function bar(): string[] {
	// mystery function that may return early...

	return myStrings
}
```
- TypeScript will check that `myStrings` is an array of strings, since that is what we explicitly said we want to return

### Static vs Dynamic
- static type systems have you write types in your code, TypeScript is a static type language, and the types are checked during build time
- Dynamic type systems perform their type checks at runtime, JavaScript is a dynamically typed language. 

### Nominal vs Structural 
- Nominal Type Systems are all about Names, lets look at this Java example: ![[Pasted image 20230225112238.png]]
- in the code above, when considering the question of type equivalence on the last line, all that matters is whether `myCar` is an instance of the class `Car` 
- TypeScript is a structural type system 
- This is because a lot of JavaScript is written without classes a lot of the time
- Structural type systems just care about shape, which works well for JavaScript code
```typescript
class Car {
	make: string
	model: string
	yea: number
	isElectric: boolean
}
class Truck {
	make: string
	model: string
	yea: number
	towingCapacity: number
}
 const vehicle = {
	 make: "Honda", 
	 model: "Accord",
	 year: 2017
 }

function printCar(car: {
	make: string
	model: string
	year: number
}) {
	console.log(`${car.make} ${car.model} (${car.year})`)
}
printCar(newCar()) // fine
printCar(newTruck()) // fine
printCar(vehicle) // fine
```
- The function `printCar()` does not care about which constructor the argument came from, only that it has:
	- A `make` property that is of type `string`
	- A `model` property that is of type `string`
	- A `year` property that is of type `number`
- If all those requirements are met, `printCar()` is happy

### Duck Typing
- Gets its name from the "Duck Test" 
> [!Quote] 
> "If it looks like a duck, swims like a duck, and quacks like a duck, then it probably is a duck"
- Very similar to Structural Typing but "Duck Typing" is usually used to describe a dynamic type system

### Strong vs Weak Type
- No real consensus around these terms
- Strong type often references static typing
- Weak type often refers to dynamic typing

## Union and Intersection Types
- Union and intersection types can be thought of as logical boolean operators (`and`, `or` )
- a union type can be thought of as `or` for types
- intersection types can be thought of as `and` for types
![[Pasted image 20230225134846.png]]
- If we wanted something that was `Fruit OR Sour` then we could include this whole diagram as options
- but if we wanted something that was `Fruit AND Sour` then we could only use `Lemon, Lime, Grapefruit` as options

### Union Types in TypeScript
- Can be described using the pipe operator ` | ` 
- for example if we had a function `flipCoin()` that can return `"heads"` or `"tails"`  we can specify it like so: 
```typescript
function flipCoin() : "heads" | "tails" {
	if (Math.random() > 0.5) return "heads";
	return "tails";
}

const outcome = flipCoin()
```
![[Pasted image 20230225140337.png]]
- as you can see the outcome is typed as either `"heads"` or `"tails"` 
- We can make this a little more complicated by using a tuple that is structured as follows: 
	- `[0]` either `"success"` or `"failure"`
	- `[1]` something different, depending on the value found in `[0]` 
		- `"success"` case: a piece of contact info `{ name: string; email: string; }` 
		- `"error"` case: an `ERROR` instance
- we will decide what happens based on our 50/50 coin flip ![[Pasted image 20230225140836.png]]
- We can have 2 types of tuples that outcome can be, and we will know what will be in the second position of the tuple based on what the string in the first position is
- We can destructure the tuple and see what TypeScript has to say about its members ![[Pasted image 20230225153648.png]]
-  When a value has a type that includes a union, we can only access the common behavior that is guaranteed to be there ![[Pasted image 20230225154022.png]]
- as we can see, the first value will always be a string, so we can access any behavior that a string could
- but on the `second` value, it can be an object or an `ERROR`. It so happens that `ERROR` also has a property of `name` so we are able to access that property alone, since it is the only common behavior between the types `second` could be

## Narrowing With Type Guards
- Ultimately we need to "separate" the two potential possibilities for our value or else we won't be able to get far
- We can do this with type guards 
>[!tip] Type Guards are expressions, which when used with control flow statements, allow us to have a more specific type for a particular value
- 



