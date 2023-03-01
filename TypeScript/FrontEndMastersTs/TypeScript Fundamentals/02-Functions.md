---
tags:: typescript
site:: frontendmasters
creator:: MikeNorth
---
- We dealt with function argument and return types, but there are a few more in-0depth features we need to cover

## Callable Types
- Both Type Aliases and interfaces offer the capability to describe call signatures: ![[Pasted image 20230301133147.png]]
- Let's make note of the following: 
	- The return type for an interface is `:number` and for the type alias its `\=> number`  (backslashes to prevent error in obsidian) 
	- Because we provide types for the functions `add` and `subtract`, we don't need to provide type annotations for each individual function's argument list or return type

### `void` 
- Sometimes functions don't return anything, and we know from experience with JavaScript what actually happens in the situation below is that `x` will be `undefined` : ![[Pasted image 20230301133650.png]]
- So why is it showing up as `void`? 
- `void` is a special type, that's specifically used to describe function return values. It has the following meaning: 
> [!info] 
> The return value of a void function is intended to be ignored
>
>
- We could type functions as returning `undefined`, but there are some interesting differences that highlight the reason for `void`'s existence: ![[Pasted image 20230301133945.png]]
-  It happens that `Array.prototype.push` returns a number, and our `invokeInFourSeconds` function above is unhappy about this being returned from the callback

## Construct Signatures
- Construct Signatures are similar to call signatures, except they describe what should happen with the `new` keyword ![[Pasted image 20230301134139.png]]

## Function Overloads
- Imagine the following situation: ![[Pasted image 20230301134213.png]]
- What if we had to create a function that allowed us to register a "main event listener" ?
	- if we passed a `form` element, we should allow registration of a "submit callback" 
	- If we are passed an `iframe` element, we should allow registration of a "`postMessage` callback"  
- Lets give it a shot: ![[Pasted image 20230301134934.png]]
- This is not good, we are allowing too many possibilities here, including things we don't aim to support (using a `HTMLIFrameElement` with `FormSubmitHandler`, which doesn't make much sense)
- We can solve this using function overloads, whre we define multiple function heads that serve as entry points to a single implementation:  ![[Pasted image 20230301135150.png]]
- Now we have effectively created a linkage between the first and second arguments, which allows our callbacks argument type to change, based on the type of `handleMainEvent`'s first argument.
