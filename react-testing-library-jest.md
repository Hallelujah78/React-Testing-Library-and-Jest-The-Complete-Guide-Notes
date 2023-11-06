# React Testing Library and Jest: The Complete Guide

- course commenced 6/11/23 - 20:27pm.

## 1. How to Get Help

- just intro

---

## 2. Join Our Community!

- intro stuff

---

## 3. Start Testing... Now!

- [basic start project](http://www.codesandbox.io/s/rtl-starter-sq54b4 "starter project where we can start writing tests straight away")
- outline core functionality of the app
  - app starts up
  - 6 products visible
  - click load more button
  - 12 products visible
- test 1
  - when app starts, we see six products
- test 2
  - if we click load more, we see 12 products
- steps
  - create App.test.js in src
    - our App.test.js:

```js
import { render, screen, waitFor } from "@testing-library/react";
import user from "@testing-library/user-event";
import App from "./App";

test("shows 6 products by default", async () => {
  render(<App />);
  const headings = await screen.findAllByRole("heading");
  expect(headings).toHaveLength(6);
});
test("clicking on the button loads 6 more products", async () => {
  render(<App />);

  const button = await screen.findByRole("button", {
    name: /load more/i,
  });
  await user.click(button);
  await waitFor(async () => {
    const headings = await screen.findAllByRole("heading");
    expect(headings).toHaveLength(12);
  });
});
```

- to run the tests in codesandbox, click the 'tests' tab above the preview window
  - tests will run automatically or you can click on the play button
  - testing in codesandbox is buggy so you may have to refresh the page to get it to work
  - both of the tests pass

---

## 4. A few critical questions

- What were all those import statements?
- How were our tests found?
- What did all the testing code do?

<img alt="" width="653px" height="366px" src="image.png">

- @testing-library/react
  - takes component and renders it
  - uses ReactDOM
- user-event
  - simulate user input
- @testing-library/dom
  - automatically included in our project by testing-library/react
  - helps us find our elements
- jest

- how were our tests found?
  - Jest finds files in the src folder that
    - end with .spec.js
    - end with .test.js
    - are placed in a folder called `__test__`
    - they just have to satisfy one of these criteria
    - it assumes they contain tests
      - jest will automatically execute them
- what did the testing code do?
  - we call the builtin 'test' function
    - describes the test and calls a callback function that contains testing code
    - `render(<App/>)` renders the app component
    - used `screen` to find all elements that have the `heading` role
      - find out more about roles later
    - `expect(headings).toHaveLength(6)` - there should be 6 headings
      - anything other than 6 fails the test

## 5. Project Setup

- what we'll do now
  - make a simple project and test it
- later
  - we'll look at a large prebuilt project with complex features, and we'll test and add new features
- the simple project
  - form where user can enter in information (name, email)
  - the user can click submit and the details are added to a list of users
- Stephen is using create-react-app

  - I'll try to follow along with Vite
  - create a new vite React project called 'users'

  ## 6. Quick Note

  - filler

  ## 7. Adding the Form

  - see lesson 10 - completed users project

  ## 8. Handling User Input

  ## 9. Rendering the List of users

## 10. Completed Users Project

## 11. Our first test

## 12. Element Query System
