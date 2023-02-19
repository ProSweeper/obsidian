### Arrays

-   ordered sequence of elements that can be different data types or the same
-   special kind of object in javascript
-   Array Functions:
    -   `push()` add an element to the end of an array
        
    -   `pop()` remove an element from the end of an array and return it
        
    -   `shift()` remove an element from the start of an array and return it
        
    -   `unshift()` inserts an element into the beginning of an array
        
    -   `delete` keyword deletes an element but leaves undefined holes behind. use `pop()` or `shift()` instead
        
    -   `reverse()` reverses an array
        
    -   `splice()` add or remove elements
        
        -   first param is the index number where the new element should be spliced in
        -   second param is the number of elements that should be removed, if none are to be removed put 0 as the second param
        -   rest of parameters are the new elements to be added in
        
        ```jsx
        // starting at the 3rd index (0 base) remove one element and then add 
        // the elements 'ten' and 8
        var array = [1, 3, 5, 'seven', 10]
        array.splice(2,1,'ten',8);
        console.log(array); // [1, 3, 'ten', 8, 'seven', 10] 
        
        ```
        
    -   `slice(0)` used to slice out a piece of an array, returns that piece as a new array
        
        -   first param is the index number to start slicing
        -   second param is the index number up to which the array should be sliced out (non-inclusive). If the second param is left empty the slice will be an array of the first parameter index up to the end of the array
    -   `concat()` concatenates arrays and creates a new array, can be any number of arrays
        
    -   `for...of` statement: creates a loop to iterate over iterable objects like arrays and array like objects
        
        ```jsx
        var array = [1,'two',3,4,5, 'six','seven',8,'nine',10]
        
        for(let ele of array){
          console.log(ele)
        }
        ```
        

### `var` vs `let` vs `const`

-   `const` block-scoped, cannot be redeclared or reassigned
-   `var` global scoped variables, best to avoid using
-   `let` block-scoped, variables cannot be redeclared but can be reassigned