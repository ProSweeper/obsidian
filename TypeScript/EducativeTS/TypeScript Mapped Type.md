### What is a Mapped Type
- allows for creating a new type from an existing one
- using the term map refers to pointing existing members to a new type by a custom logic
    - a good example is to have an existing interface keep all the same members, but make them optional or read-only

### Advantages of the Mapped Type
- Less use of interface changes
    - if we need an interface to create a read only and a normal interface we would need to define 2 interfaces
        ```tsx
interface OriginalInterface {
		x: number;
		y: string;
}

interface ReadOnlyOriginalInterface {
		readonly x: number;
		readonly y: string;
}

let variable1: OriginalInterface = { x: 1, y: "2" };
let variable2: ReadOnlyOriginalInterface = { x: 1, y: "2" };
variable1.x = 2; // Can alter the value
variable2.x = 2; // Cannot alter the value
        ```
- Without the mapped type every interface requires a separate function to perform the transformation
- with the mapped type you only need to your transformation function to defined your mapped type
    - with the help of generic the transformation function is self sufficient, taking a generic interface and returning the mapped type definition
    - simply put, a generic is a reusable way to change the type of an interface
        ```tsx
// The interface that define a structure that allows re-assignment of values 
interface OriginalInterface {
		x: number;
		y: string;
}

// The interface that defines a structure that allows assignment
// only at creating
interface ReadOnlyOriginalInterface {
		readonly x: number;
		readonly y: string;
}

let variable1: OriginalInterface = { x: 1, y: "2" };
let variable2: ReadOnlyOriginalInterface = { x: 1, y: "2" };

// A function that transform one object to the other type
function mapOriginalInterfaceToBeReadOnly
(o: OriginalInterface): ReadOnlyOriginalInterface {
		return o;
}

let variable3 = mapOriginalInterfaceToBeReadOnly(variable1);
// variable3.x = 2;
        ```
    -   Neither of these examples above used the mapped approach

### Immutable Data with Readonly

-   The `object.freeze` function
    -   the role of this function is to take a type and return everything as read-only
    -   function will have a return type of `Readonly<T>` where `<T>` is the interface to freeze
    -   The name ‘mapped’ comes from that within the type there is an instruction keyword `in` which will loop through all the object properties and prototype chains
        ```tsx
// The interface that we want to have readonly without defining another interface
interface OriginalInterface {
		x: number;
		y: string;
}

// The mapped type that map a generic type with the readonly constraint
type ReadonlyInterface<T> = { readonly [P in keyof T]: T[P] };
        ```
    -   The return type is not the original generic type `T` passed, but an aggregate of all properties
### Adding Generic Using Mapped

```tsx
// The interface that we want to have readonly without defining another interface
interface OriginalInterface {
    x: number;
    y: string;
}

// The mapped type that map a generic type with the readonly constraint
type ReadonlyInterface<T> = { readonly [P in keyof T]: T[P] };

// Function that convert the object from one type to another
function genericInterfaceToReadOnly<T>(o: T): ReadonlyInterface<T> {
    return Object.freeze(o);
}

const original: OriginalInterface = { x: 0, y: "1" };
const originalReadonly = genericInterfaceToReadOnly(original);
// originalReadonly.x = 3; // error TS2540: Cannot assign to 'x'
```

-   The mapped type `ReadonlyInterface<T>` is a one line definition that loops through all the properties (`P` above) of `T` and returns a read only version of that property
-   We do not need to create this interface since TypeScript comes with a set of predefined mapped types