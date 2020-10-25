---
title: Javascript The New Hard Parts
---

## What Happens When Javascript Runs My Code?

- Thread of execution (parsing and executing code line by line)
- Global variable environment is created
- Functions are garbage collected if they're no longer being used
- Synchronous JS is single threaded
- Top of the call stack is whatever code that's currently running
- Push: adding to a stack
- Pop: removing from a stack

```jsx
// Constant num is declared and set to 3 in memory
const num = 3

// Function is declared and set in memory
function multiplyBy2 (inputNumber) {
  // Function body isn't parsed until it's called
  const result = inputNumber * 2
  return result
}

// Constant name is declared and set to 'Will' in memory
const name = 'Will'
```

### Invoking a Function

```jsx
// Constant num is declared and set to 3 in memory
const num = 3

// Function is declared and set in memory
function multiplyBy2 (inputNumber) {
  // Function body isn't parsed until it's called
  const result = inputNumber * 2
  // return value of result into next execution context
  return result
}

/*
  New execution context is created with thread of execution
  and local variable environment
*/
const output = multiplyBy2(4)
const newOutput = multiplyBy2(10)
```

## Introducing Asynchronicity

Goals:

- Be able to do tasks that take a long time to complete
- Continue running code without one task blocking execution
- When slow task is complete, run functionality that relies on that data

```jsx
// Solution 1
// display function is declared
function display (data) {
  console.log(data)
}
const apiData = fetch('<https://twitter.com/api/getTweets>')
// No interactivity until API call is resolved
display(apiData)
console.log('Last')
```

## Introducing Web Browser APIs/ Node Background Threads

- setTimeout is facade function: web browser feature
- web browser features:
    - Timer (what setTimeout is under the hood)
- JS call stack must be cleared before setTimeout runs
- Browser features go to callback queue
    - Functions ready to go to call stack

```jsx
// Solution 2

// printHello function is declared
function printHello () {
  console.log('hello')
}
setTimeout(printHello, 1000)
console.log('First')
```
