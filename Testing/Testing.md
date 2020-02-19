# Testing

## Why Test?

-  Testing gives you confidence in your code
-  Even though it may take some up front time testing can save you a lot of time in the long run because you don't have to manually test your code and if you implement a new feature you can see what affects it might have had on the codebase

Starting with [Kent C Dodds' Testing JavaScript Course](https://testingjavascript.com/)

-  The simple form of testing is saving the result of a function in a variable and testing if it is equal to the expected result of the function
   -  if result !== expected then we can throw a new Error
-  The job of a testing framework is to make that error as useful as possible so you can quickly determine and fix the problem
   example test with component FavoriteNumber

```
import React from 'react'
import ReactDOM from 'react-dom'
import {FavoriteNumber} from '../favorite-number'

test('renders a number input with a label "Favorite Number"', () => {
    const div = document.createElement('div')
    ReactDOM.render(<FavoriteNumber/>, div)
    expect(div.querySelector('input').type).toBe('number')
    expect(div.querySelector('label').textContent).toBe('Favorite Number')
})
```
