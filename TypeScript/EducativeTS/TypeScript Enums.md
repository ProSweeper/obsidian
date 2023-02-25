### Enum With and Without Values

-   An `Enum` is a structure that proposes several allowed values for a variable
    
    -   it is a way to constrain variable values by defining specific possible entries
-   `enum` with values
    
    -   can be of a single type such as string
        
        ```tsx
        enum MyStringEnum {
            ChoiceA = "A",
            ChoiceB = "B",
        }
        ```
        
    -   can be a mixed typing (this is less recommended)
        
        ```tsx
        enum MyStringAndNumberEnum {     
            ChoiceA, // 0     
            ChoiceB = "B",     
            ChoiceC = 100 
        }
        ```
        
-   `enum` without values
    
    -   `enum` is a type that enforces a limited and defined group of constants
        
    -   `enum` must have a name and accepted values, afterwards you can use the `enum` as a type
        
    -   if the values are not defined like below, then the values are all constant starting from `0` for the first term and increasing by one until the end
        
        ```tsx
        enum MyEnum2 {
            ChoiceA, // 0
            ChoiceB = 100, // 100
            ChoiceC, // 101
            ChoiceD = MyEnum.ChoiceC, // 2
        }
        ```
        
    -   `enums` can be set directly with a constant or by using computation
        
        -   a computed constant is a value provided from another `enum` or a value computed by addition/subtraction/multiplication/division/modulo/’or’/’xor’

### Accessing Enum Values

-   a variable set with an `enum` that has a `number` lets access the `enum` name from the integer
    
-   an `enum` with string values does not have this capability
    
    ```tsx
    enum Orientation {
        East,
        West,
        North,
        South,
    }
    let directionInNumber = Orientation.East; // Access with the Enum
    let directionInString = Orientation[0];  // Access the Enum string from number
    console.log(directionInNumber); // 0
    console.log(directionInString); // East
    ```
    
-   as mentioned above accessing a string `enum` from a number does not work below
    
    ```tsx
    enum OrientationString {
        East = "E",
        West = "W",
        North = "N",
        South = "S",
    }
    let directionInNumber = OrientationString.East; 
    // Access with the Enum
    
    let directionInString = OrientationString[0];  
    // Access the Enum string from number
    
    let directionInString2 = OrientationString["E"];  
    // Access the Enum string from number
    
    console.log(directionInNumber); // E
    console.log(directionInString); // will not compile 
    console.log(directionInString2); // will not compile
    ```
    
-   the bottom code examples are how the typescript compiles into javascript
    
    ```tsx
    enum Orientation {
        East,
        West,
        North,
        South,
    }
    // compiles into
    let Orientation;
    (function (Orientation) {
        Orientation[Orientation["East"] = 0] = "East";
        Orientation[Orientation["West"] = 1] = "West";
        Orientation[Orientation["North"] = 2] = "North";
        Orientation[Orientation["South"] = 3] = "South";
    })(Orientation || (Orientation = {}));
    ```
    

### Merging and Adding Functionality to Enum

-   like interfaces, `enum` can be defined in more than one place
    
    -   you can define an `enum` again after it is initially defined
        
    -   in the end all values merge into a single `enum`
        
    -   however, the first value of every `enum` must have an explicit value
        
    -   if an explicit value is defined twice, only the last value will be associated with the `enum`
        
    -   listing the same value twice is not a feature of multiple definitions
        
        ```tsx
        enum EnumA {
            ChoiceA,
        }
        enum EnumA {
            ChoiceB = 1,
        }
        
        let variable1: EnumA = EnumA.ChoiceA;
        console.log(variable1); // 0
        variable1 = EnumA.ChoiceB;
        console.log(variable1); // 1
        ```
        
-   another feature of `enum` is that you can attach functions that will be accessible statically by the `enum`
    
    -   using an`enum` with a function means you can use `Orientation.East` as well as `Orientation.yourFunction`
    -   however, defining a function inside an `enum` requires use of a namespace with an exported function