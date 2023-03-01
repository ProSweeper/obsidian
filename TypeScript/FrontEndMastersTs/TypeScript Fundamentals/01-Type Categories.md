---
tags:: typescript
site:: frontendmasters
creator:: MikeNorth
---
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
- Continuing with our maybe get user info function from above we can see the following: ![[Pasted image 20230228131112.png]]
- TypeScript has a special understanding of what it means when our `instanceof` check returns `true` or `false`, and creates a branch of code that handles each possibility

### Discriminated Unions 
![[Pasted image 20230228131330.png]]
- In the first case TypeScript can be seen to understand that if the first string is `"error"` then the second element has to be `Error` 
- TypeScript understands that the first and second positions of our tuples are linked. What we are seeing here is sometimes referred to as a discriminated union type

### Intersection Types in TypeScript
- Intersection Types in TypeScript can be described using the `&` operator
- For example, what if we had a `Promise` that had extra `startTime` and `endTime` properties added to it? ![[Pasted image 20230228131603.png]]
- This is different than what awe saw with union types, this is quite literally a `Date` and `{ end: Date }` mashed together, and we have access to everything we need immediately
- It is far less common to use intersection types compared to union types

## Interfaces and Type Aliases
- TypeScript provides two mechanisms for centrally defining types and giving them useful and meaningful names: **Interfaces** and **Type Aliases**. 
- Often you are free to choose to use either an interface or a Type Alias

### Type Aliases
- Allow us to give a friendly name to our types
- Think back to the ` : {name: string, email: string}` syntax we have used up until this point for type annotations. 
- this syntax will get increasingly complicated as more properties are added to theis type. 
- Furthermore, if we pass objects of this type around through various functions and variables, we will end up with a lot of types that need to be manually updated whenever we need to make any changes
- Type aliases help to address this by allowing us to: 
	- define a more meaningful name for this type
	- declare the particulars of the type in a single place
	- import and export this type from modules, the same as if it were an exported value ![[Pasted image 20230228133420.png]]
- We can see a couple of things here: 
	- the tooltip on `info` is now a lot cleaner and more semantic (meaningful in connection with the concept behind it)
	- import/export of this `type` works just as it would for a normal class in JavaScript
> [!note] 
> It is important to realize that the name `UserContactInfo` is just for our convenience. This is still a structural type system, all the alias is doing is giving a name to that structure, all that matters is that the structure lines up
> 

![[Pasted image 20230228141602.png]]
- Lets look at the declaration syntax for a moment: 
```TypeScript
type UserContactInfo = {
	name: string
	email: string
}
```
- There are a few things to point out here: 
	- this is a rare occasion where we see the type info on the right hand side of the assignment operator
	- We are using `TitleCase` to format the alias' name. This is a common convention 
	- As we can see below, we can only declare an alias of a given name once within a scope. This is kind of how a `let` or a `const` variable declaration works 
		- this limitation does not exist for interfaces
	- ![[Pasted image 20230228141905.png]]
- A type alias can hold any type, as it's literally an alias (name) for a type of some sort 
- Here is an example of how we can clean up the code from our union and intersection types we used above: ![[Pasted image 20230228142109.png]]
- we can see how much nicer the return type looks in this case

### Inheritance in Type Aliases
- you can create type aliases that combine existing types with new behavior by using Intersection (`&`) types ![[Pasted image 20230228142246.png]]
- `specialDate` has everything `Date` has, and on top of that it is doing some other stuff
- While there is no true `extends` keyword, this can be used when defining aliases to a similar effect

### Interfaces
- An interface is a way of defining an **Object Type**. An object type can be thought of as "an instance of a class could conceivably look like this"
- Interfaces are more limited than Type Aliases since they can only be used to define Object Types
- For example `string | number` is not an object type, because it makes use of the union operator
- An interface can be used to describe something within a union operator though ![[Pasted image 20230301122306.png]]
- Like Type Aliases, interfaces can be imported/exported between modules just like values, and they serve to provide a "name" for a specific type

### Inheritance in Interfaces
- Inheritance has a lot more formality around inheritance with interfaces
- If you have ever seen a JavaScript class that "inherits" behavior from a base class, you've seen an example of what TypeScript calls a **heritage clause** ![[Pasted image 20230301122523.png]]
- `extends` is used to describe inheritance between like things 
	- classes can extend from classes
	- interfaces can extend from other interfaces
- `implements` is used to describe inheritance between unlike things
	- when working between interfaces and classes you need to use `implements` 
	- Ex: The class `Dog` Implements the interface `AnimalLike`
- Just as in JavaScript, a subclass `extends` from a base class
- Interfaces are a great way to define "contracts" between things
- additionally a sub-interface `extends` from a base interface, as shown below: ![[Pasted image 20230301122632.png]]
- TypeScript adds a second heritage clause that can be used to state that a given class should produce instances that confirm to a given interface: `implements` ![[Pasted image 20230301122754.png]]
- In the above example we can see that TypeScript is objecting to us failing to add an `eat()` method to our `Dog` class. 
- Without this method, instances of `Dog` do not conform to the `AnimalLike` interface ![[Pasted image 20230301122913.png]]
- Now `Dog` conforms to `AnimalLike` 
- While TypeScript does not support true multiple inheritance (extending from more than one base class), this `implements` keyword gives us the ability to validate, at compile time, that instances of a class conform to one or more "contracts" (types). 
	- it is probably a good thing that we cannot have official multiple inheritance since things can get very complicated
- Note that both `extends` and `implements` can be used together: ![[Pasted image 20230301123140.png]]
- we can see the `Dog` is extending another class (`LivingOrganism` ) and is implementing an interface (`AnimalLike` and `CanBark`)
- While it is possible to use `implements` with a  type alias, if the type ever breaks the "object type" rules there is some potential for problems ![[Pasted image 20230301123239.png]]
- For this reason it is best to use interfaces for types that are used with the `implements` heritage clause  

### Open Interfaces
- TypeScript interfaces are "open", meaning that unlike in type aliases, you can have multiple declarations in the same scope: ![[Pasted image 20230301123405.png]]
- These declarations are merged together to create a result identical to what you would see if both the `isAlive` and `eat` methods were on a single interface declaration
- It does not matter that we are declaring the second `AnimalLike` interface lower in the code. This change will persist throughout the code base
- This can be helpful in certain situations: 
	- imagine you wanted to add a global property to the `window` object ![[Pasted image 20230301123529.png]]
	- What we have done here is augment an existing `Window` interface that TypeScript has set up for us behind the scenes
	- Maybe there is a feature that was not available when you originally wrote the code, but you now want to tack it on to your interface. This augmentation allows us to do that

## Choosing Which to Use
- In many situations, either a `type alias` or an `interface` would be interchangeable, but...
	- If you need to define something other than an object (e.g. use of the `|` union type operator), you must use a type alias
	- If you need to define a type to use with the `implements` heritage term, it is best to use an interface
	- If you need to allow consumers of your types to augment them, you must use an interface

## Recursion
- Recursive Types are self referential and are often used to describe infinitely nestable types.
- For example consider the infinitely nestable arrays of numbers: ![[Pasted image 20230301124718.png]]
- You may read or see things that indicate you must use a combo of `interface` and `type` for recursive types
- As of TypeScript 3.7 this is now much easier and works with either type aliases or interfaces![[Pasted image 20230301124821.png]]
   