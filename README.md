# Hedgehog-Script-Style-Guide


## Here we will cover ... 
... several different points to guide coding style when contributing to Hedgehog Lab/Hedgehog Computing. This is in hopes to maintain readability and consistency as the libraries grow. 

JavaScript is a very flexible language with many design paradigms. We encourage unique personal styles to be mixed into code, however, we must maintain consistency on some things.

## `Comments`

Comments are extremely useful for readability, whether it's the maintainer or a developer or even user looking at the implementation:
<ul>
  <li>Comments explaining pieces of code that aren't self evident at a glance. Some examples are commenting on the functionality in a loop, commenting on the loop bounds if necessary, and just general functionality of code.</li>
  <li> Not every line of code needs to be commented about, just the important stuff. </li>
  <li> A comment block for each function with the following style: </li>  
</ul>

```js
/**
* @author author_name
* @param param_1 - brief explanation, mentions type(s)
* @param param_2 - brief explanation, mentions type(s)
* ... (for however many parameters you have)
* @returns - brief explanation, mention what type(s) is returned
*
* Brief function description - this function does this.
*/
```

## `File names`

We've been using lowercase & underscores for file names. Everything such as the variables and sub functions are up to you, but please name the functions/classes and file names like the following:
```js
function function_name(input) {

}
... --> named 'function_name.hhs'
```

## `Var - And Avoiding it `

We ask that you avoid `var` and always use `let` for the following reasons: 


## Not modifying original input

This one is less of a style issue and more of a functionality issue, however, it's important to address. Please make sure there's no modifications to references of original input and if there is, it needs to be cloned/deep cloned. We have a function named `deep_copy(input)` but there are other methods. 

To illustrate this example consider this block of code:

```js
function example(input) {
    
    //suppose for this example we're expecting a 2d javascript array and we've checked it
    
    let a = input;
    
    a[0][0] = 'hello';
    
    return a;

}
```


```
If we do the following, we get this...

let x = [[2,3],[4,5]];
print(x) --------> 2,3,4,5
let y = example(x);
print(y) ---------> hello,3,4,5
//but now the issue is:
print(x) ---------> hello,3,4,5
//.. we modified the user's original input through the function which we want to avoid
```
There are multiple solutions to this. I'll demonstrate 4 through the usage of transpose.hhs which copies the original input and modifies it:

```js

*import math: deep_copy

function transpose(input) {

  //input type is TRUE if its a Mat object otherwise, false. We're only accepting JS arrays and Mat objects.
  let input_type = (input instanceof Mat);
  
  //if input_type is TRUE, raw_input = input.clone() which suffices for Mats, as there is a built in function in the base code for cloning Mats
  //if FALSE, then we need to deep copy the input as a shallow one wont suffice and is only a reference. One can use deep_copy() for that. 
  //Don't forget to import math:deep_copy
  let raw_input = (input_type) ? input.clone() : deep_copy(input);
  
  //... the rest of the function


}
```

That's 2 ways: using deep_copy(input) and input.clone() (for Mats). 

The other 2 ways are as following:

```js
//works for variety of inputs
let raw_input = JSON.parse(JSON.stringify(input));
```

or... perhaps the simplest way...

```js
//works only for JavaScript arrays, as this changes them into Mats and then changes them back so we can modify them with JS functions
let raw_input = mat(input)
//do this to change it back to JS array. It no longer is a reference to input. 
raw_input = raw_input.val;
```

## Formatting

Whichever editor you use, hopefully it has a autoformat option. For example Visual Studio on mac has Command+K+F to format the selected code. We just ask you do this, and if your editor doesn't support this, try your best to keep formatting consistent and clean.



## Thank you!

Thank you for reading this and thank you for contributing.

