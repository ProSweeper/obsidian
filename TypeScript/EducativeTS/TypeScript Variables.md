### Declaring Variables
- Much the same as JavaScript
    - same scopes and behavior from `let` and `const` declarations
    - `const` objects and arrays can have their properties modified
    -  this is because the reference is not changing, you can change the values inside an object

### Declaring Types in untyped code
-   keyword `declare` can be used before `let` or `const`
-   it declares to TS that the variable is somewhere but not saying where
-   Officially called Ambient Declaration
-   Will lose all intellisense however
-   common use case is a library that exists in JavaScript but has not migrated to typescript

### Hoisting Variables
- JS quirk that brings all declarations made with `var` to the top of the function
- not available with `let` and `const`
### TypeScript Scope

- Shadowing Scope: when one variable is declared twice, in an outer scope and an inner scope
    -   ex: 2 loops and both use the variable `i`
        ```tsx
        for (let i = 0; i < 10; i++) {
        	for (let i = 0; i < 5; i++) {
        		console.log('hello');
        	}
        }
        ```
    - typescript is smart enough to know they are 2 separate variables but this is confusing and prone to error
- Capturing Scope: when you have a variable defined in an inner scope and then used inside a function that you assign to another scope. During the assignment of the function, the variable defined in the inner scope will be captured (like a snapshot)
    ```tsx
    function mainFunction() {
        let innerFunction;
        if (true) {
            // Environment for capturing start here
            let variableCapturedByTheInnerFunction = "AvailableToTheInnerFunction";
            innerFunction = function() {
                return variableCapturedByTheInnerFunction;
            }; // Environment for capturing stop here
        }
        return innerFunction();
    }
    console.log(mainFunction());
    ```
- `let` and `const` can be declared once per scope.
- you can have several variables of the same name, they just cannot be in the same scope

### Switch Scope
- a  `switch` statement is similar to an `if` statement
- the condition set between the 2 parentheses must be met to reach one of the cases, else the default case will be entered
    ```tsx
    function switchFunction(a: number): void {
        switch (a) {
            case 1: {
                let variableInCase1 = "test";
                console.log(variableInCase1);
                break;
            }
    				case 2: {
                let variableInCase2 = "test2";
                console.log(variableInCase2);
                break;
            }
    				default: {
              console.log("Default");    
    		    }
    			}
    		}
    ```
    - not adding the `break` keyword will result in the next case running
- It is best practice to encapsulate each case with curly brackets to limit the scope of each case to itself

### Declaring a String Variable
-   same as JS, Known as implicit declarations
    -   single or double quotes (also back ticks for interpolations)
    -   TS standards are double quotes however
-   `"\\n"` creates new line, just like JS however you do not need it in TS
    
    -   both below methods work the same, however the second method requires back ticks
    
    ```tsx
    let multiline1 = "Line1\\n" + "Line2\\n" + "Line3\\n";
    
    let multiline2 = `Line1 
    Line2 
    Line3`;
    ```
    
-   Explicitly declaring a string: just type `: string` after the variable name
    
    ```tsx
    let myString: string ="I am a string";
    let myString2: string = `so am I`;
    ```
    

### Declaring a Number Variable

-   number type defines the type for `interger` `float` `double` ect…
    -   can also be assigned with `NaN`

### Booleans, Functions, Objects

-   Boolean: `let b: boolean = true;`
    -   `0` and `1` cannot be assigned to a Boolean, it will not compile

### Avoid `any` at Any Time Possible

-   Does not enforce protections
-   limits readability

### Understanding and Using the Void Type

-   the `void` type cannot hold any data (can only be `undefined` or `null` if `strictNullChecks` compiler option is turned off)
    
-   When to use:
    
    -   useful for function return types and can be explicitly defined on functions after the parameters parentheses
    
    ```tsx
    function logMessage(message: string): void {
      console.log(message);
    }
    ```
    

### Mutable and Immutable Arrays

-   Arrays operate very similar in TS as JS
    -   defined by putting a pair of square brackets after the type declaration
        ```tsx
        let a: number[];
        let b: string[];
        ```
    -   can also be implicitly typed
    -   Good for maintainability if you use explicit typing
    -   without explicitly typing you leave the array able to have a different type added to it late on
    -   Arrays can have multiple types
        
        ```tsx
        let multipleTypeArray = [1, true, 3]; 
        // Same as:
        let multipleTypeArrayExplicit: (number | boolean)[] = [1, true, 3];
        ```
        
        ```tsx
        let myArray = new Array<number>();
        printArray(myArray);
        
        // Is the same as:
        let myArray2: Array<number> = [];
        printArray(myArray2);
        
        // Is the same as:
        let myArray3: number[] = [];
        printArray(myArray3);
        
        function printArray(a: number[]): void {
            console.log(`Before: ${a}`);
            a.push(1);
            console.log(`After: ${a}`);
        }
        ```
        
-   TS allows immutable arrays
    
    -   can be declared in 2 manners:
        
        ```tsx
        let list: ReadonlyArray<number> = [1, 2];
        ```
        
        ```tsx
        let list: readonly number[] = [1, 2];
        ```
        
    -   Read only arrays do not allow any mutation to occur to the array or its values
        

### Undefined VS Null

-   `undefined` : a variable that is declared but not initialized.
    
-   TS must be made stricter in order to prevent it from assigning null or undefined to every type
    
    -   set TS `strictNullChecks` option to `true`in the compiler settings
-   the `union` or question mark is then used to optionally define the variable
    
    -   the union uses the pipe character inbetween the main type and null
        
        ```tsx
        let num: number | null | undefined = 123;
        ```
        
    -   the question mark is placed after the name of the variable
        
        ```tsx
        function action(a:number, b?:number) {}
        ```
        
    -   the question mark allows for a value to be absent, the `union` requires a value, even if it is `undefined` or `null`
        
    -   when using the question mark, you cannot allow a non-undefined parameter to follow in a function signature:
        
        ```tsx
        function action(a:number?, b:number) {} 
        // will not compile
        ```
        
-   `null` should only be used to represent the absence of something
    

### Unknown

-   `unknown` type allows us to set a wide variety of types without allowing unwanted access to properties or the value of a type
    
-   if a variable is type `unknown` the only way to access it is using type assertion
    
    ```tsx
    let variable3: unknown;
    variable3 = "It is a string";
    let variable3String = variable3 as string;
    ```
    
-   this should be avoided as it can lead to mistakes

### Literal Type

-   A literal type means that the value is an exact one.
    
    -   for example: a string literal of `"test"` would mean the value of that variable can only be `"test"`
-   you define a Literal Type by:
    
    ```tsx
    type MyType = ...
    ```
    
-   A string literal is a way to define a string that limits potential values to be used, moslty used with a union which allows specifying more than one string value
    
    -   if you wanted to limit the value of a variable called `direction` to the 4 compass directions this is how you would do it:
        
        ```tsx
        type Direction = "north" | "south" | "east" | "west";
        let myDirection:Direction = "north";
        // let yourDirection:Direction = "no-where"; // Does not compile
        ```
        
-   A number literal is used much the same way
    -   if you have a set of values that you want to limit a number variable to being:
        ```tsx
        type Column = 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12;
        let menuSize: Column = 4;
        let mainContent: Column = 100; // Doesn't compile because only accept 1 to 12
        ```
-   It is possible to create a literal mixed type as well

### Symbol

-   `Symbol` is a primitive type, its goal is to provide a unique and immutable variable
-   a symbol can take a parameter with a string value, and defining 2 symbols with the same parameter will produce a different symbol
    -   parameters are just there to help developers when printing the symbol to the console
-   the main difference between a symbol and a constant is that the symbol is unique
    -   this is handy when using comparison operators
        ```tsx
        let v1 = "value1";
        let v2 = "value1";
        if (v1 === v2) {
            console.log("Equal when string"); // This will print
        }
        let s1 = Symbol("value1");
        let s2 = Symbol("value1");
        if (s1 === s2) {
            console.log("Equal when symbol"); 
            // They are not equal, won't print
        }
        ```
-   a unique symbol is used to create a literal type and must be defined with the use of `const`

```tsx
let aSymbol: Symbol = Symbol("Value1");
aSymbol = Symbol("Value2"); // Type: Symbol

const aSecondSymbol: Symbol = Symbol("Value3");  // Type: Symbol
const aThirdSymbol: unique symbol = Symbol("Value3");  // Type: typeof(aThirdSymbol)
```

### Casting to Change Type

-   How to cast:
    
    -   2 different ways, using `<>` or `as`
        
    -   the former is not recommended since it conflicts with JSX/TSX
        
    -   latter is just as good and works in all situations
        
        ```tsx
        const unknownType: unknown = "123"
        const cast1: number = <number>unknownType;
        const cast2: number = unknownType as number;
        ```
        
-   Casting constraints:
    
    -   if you cast to a `string` directly without using an unknown type, TS will warn there are not sufficient overlaps
        -   this will not run
            
            ```tsx
            const str: string = "123";
            const bool: boolean = true;
            const castFromString:number = str as number;
            const castFromBoolean:number = bool as number;
            console.log(castFromString)
            console.log(castFromBoolean)
            ```
            
        -   this will run
            
            ```tsx
            const str: string = "123";
            const bool: boolean = true;
            const castFromString:number= (str as unknown) as number;
            const castFromBoolean:boolean = (bool as unknown) as boolean;
            console.log(castFromString)
            console.log(castFromBoolean)
            ```
            
-   Type assertion is when you tell TS what type an object is
    
    -   This is done often through interfaces but is better implemented through assigning a type as opposed to casting
        
        -   Better approach
        
        ```tsx
        interface ICast1 { m1: string } 
        interface ICast2 { m1: string, m2: string } 
        let icast1: ICast1 = { m1: "m1" }; 
        let icast2: ICast2 = { m1: "m1", m2: "m2" }; 
        let icast3: ICast1 = icast2; // work without cast because of the structure
        ```
        
        -   worse approach
        
        ```tsx
        interface IMyType {
          m1: string;
          m2: number;
        }
        
        let myVariable = {} as IMyType; //
        ```
        
    -   casting on object can lead to properties getting coerced into being `undefined`
        
    -   since casting tells typescript that you know what you are doing, it won’t complain
        
        -   non-optional members that are not present will be undefined even if the contract specifies the type must have that member
    -   Best Practice with casting is to do it as little as possible
        
        -   cast in a strategic manner (like when getting an untyped object)
        -   when you need ot create a new type, its better to assign a type (using explicit declaration) that to cast it
        -   doing so will provide intellisense support and transpiler protection
            -   this keeps the code stable with the expected type