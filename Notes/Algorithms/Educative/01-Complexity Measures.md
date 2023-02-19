### Comparing Algorithms

-   Criteria: Time and Space
    -   The less Time and or memory it takes an algorithm to complete its task the better
-   Comparing Execution Time
    -   Measure the amount of primitive operations executed for a given input, less is better
    <aside> 💡 A primitive operation is a simple processor instruction such as assigning to a variable or array index, reading from a variable or array index, comparing 2 variables, arithmetic operations, calling a function, ect…
    
    </aside>
    
    <aside> 💡 Displaying an array or performing a function cannot be considered primitive operations since there are multiple things going on there for each
    
    </aside>
    
    -   3 types of analysis:
        -   Best Case: input that results in fewest possible primitive operations.
        -   Worst Case: input that results in the most possible primitive operations
        -   Average Case: difficult to compute as we must know the frequencies of possible inputs
    -   Verdict: Worst case is the most useful for judging algorithms

### Measuring Time Complexity

-   A for loop with ‘n’ iterations
    
    ```jsx
    // just as an example, n can be anything
    var n = 10 
    var sum = 0
    
    // for loop
    for(var i=0;i<n;i++)
      sum+=2
    console.log(sum)
    ```
    
    -   lines 2 and 3 each have 1 primitive operation (variable assign)
        
    -   Line 6, the for loop is a little complicated
        
        -   initializing `i` is 1 operation and happens once
        -   incrementation (`i++`) is 3 operations (reading `i` adding 1, storing the result in a variable)
        -   the testing operation (`i<n`) is 3 operations (reading 2 variables and comparing them)
        -   the testing operation happens n+1 times (1 extra time for when `i` is larger than `n`)
        -   the increment happens n times since once the test fails on the nth + 1 time the loop exits and does not perform an incrementation
        -   in total line 6 has 1 + 3n + 3(n+1) or 6n + 4 operations
    -   line 7 has 3 operations (reading sum, adding to it, and saving it) and occurs n times since it happens each for loop iteration
        
    -   line 8 has 2 operations (calling the function and reading the variable)
        
        Line No.
        
        Number of Primitive Operations
        
        2
        
        1
        
        3
        
        1
        
        6
        
        6n + 4
        
        7
        
        3n
        
        8
        
        2
        
-   A program with nested for loops
    
    ```jsx
    //n can be anything
    var n = 5 
    var m = 7
    var sum = 0
    for(var i = 0; i < n; i++)
      for(var j = 0; j < m; j++)
        sum += 1
    
    console.log(sum)
    ```
    
    -   skipping the obvious we can go to line 5
        
    -   initializing a for loop has 6n + 4 operations for line 5
        
    -   line 6 is also initializing a for loop so there is 6n + 4 operations. but it happens n times as it occurs every iteration of the loop it is nested in. so line 6 has n(6m+4) operations
        
    -   line 7 happens once for each **m** and once for each _n_ and contains 3 operations (reading, adding, assigning) so it has 3mn operations
        
        Line No.
        
        No. of Primitive Operations
        
        2
        
        1
        
        3
        
        1
        
        4
        
        1
        
        5
        
        6n+4
        
        6
        
        n(6m+4)
        
        7
        
        3nm
        
        9
        
        2
        

### Asymptotic Analysis and Big O Intro

-   Generally when comparing algorithms we care only for large inputs

<aside> 💡 Asymptotic notation compares 2 functions _****f(n)****_ and _****g(n)****_ for large values of n. this aligns well with our need to compare large input sizes for an algorithm

</aside>

-   Big O Notation - an asymptotic notation
    -   We can simplify the time complexity for big O notation by dropping all constants and only focusing on the highest order term (n to the largest power)
        -   Thus, n^2+2n+1 is O(n2) while n^5+4n^3+2n+43 is O(n5)

### Other Asymptotic Notations

-   Big Omega
-   Big Theta
-   Little o
-   little omega
-   Why Big O is the preferred notation
    -   we are only really concerned with the worst case scenario in terms of time and space complexity.

### Common Complexity Scenarios

-   Simple for loop: O(n)
    
    ```jsx
    for (var x = 0; x < n; x++) {
        //statement(s) that take constant time
    }
    ```
    
-   Nested For loop: O(nm) or O(n^2)
    
    -   simple nested, inner loop runs m times and the outer loop runs n times
    
    ```jsx
    for (var=0; i<n; i++){
        for (var=0; j<m; j++){
            //Statement(s) that take(s) constant time
        }
    }
    ```
    
    -   dependent variables in a for loop
    
    ```jsx
    for (var i=0; i<n; i++){
        for (var j=0; j<i; j++){
            //Statement(s) that take(s) constant time
        }
    }
    ```
    
-   Loops with log(n) complexity
    
    -   a loop statement that multiplies or divides the loop variable by a constant
        
    -   the _i_ variable gets multiplied by 2 every iteration so as it gets bigger and bigger the loop ends faster each time
        
        -   the loop only increases in time complexity when _n_ increases by an order of 2, it will run the same amount of times when **n** is 5, 6, 7, 8 but once it hits 9 then the outer loop will be able to run once more, and the inner loop that many more times
        
        ```jsx
        // Initializations
        const n = 10;
        const pie = 3.14;
        let sum = 0;
        var i = 1;
        
        while (i < n) {
            console.log(pie);
            for (var j = 0; j < i; j++) {
                sum = sum + 1;
            }
            i *= 2;
        }
        console.log(sum)
        ```