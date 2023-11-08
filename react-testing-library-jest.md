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

  - see lesson 10 - completed users project

  ## 9. Rendering the List of users

  - see lesson 10 - completed users project

## 10. Completed Users Project

- link to download what we built in lessons 7-9

## 11. Our first test

- Test writing process

  - Pick out one component to test by itself
  - make a test file for the component if one does not exist
  - decide what the important parts of the component are
  - write a test to make sure each part works as expected
  - run tests at the command line

  - test UserForm.jsx
    - create UserForm.test.js
    - important parts are:
      - show two inputs and one button
      - entering a name and email and submitting causes onUserAdd to be called

- tests have a similar format
  - render the component
  - manipulate the component (simulate typing, etc)
  - assertion - make sure it's doing what we expect it to
- our UserForm.test.js looks like this:

```js
import { render, screen } from "@testing-library/react";
import { user } from "@testing-library/user-event";
import UserForm from "./UserForm";

test("it shows two inputs and a button", async () => {
  // render the component
  render(<UserForm />);

  // manipulate the component or find an element in it
  const inputs = screen.getAllByRole("textbox");
  const button = screen.getByRole("button");

  // Assertion - make sure component is doing what we expect it to do
  expect(inputs).toHaveLength(2);
  expect(button).toBeInTheDocument();
});
```

- incidentally, [this](https://zaferayan.medium.com/how-to-setup-jest-and-react-testing-library-in-vite-project-2600f2d04bdd "perfect guide for adding jest to a Vite react project") guide 100% works for adding Jest to a new Vite react project
- running our tests with `npm run test` we get:

```js
> users@0.0.0 test
> jest

 PASS  src/components/UserForm.test.js
  √ it shows two inputs and a button (127 ms)

Test Suites: 1 passed, 1 total
Tests:       1 passed, 1 total
Snapshots:   0 total
Time:        3.201 s
Ran all test suites.
```

## 12. Element Query System

- what happens when we run our tests?

  - executed in NodeJS environment
    - fake browser environment with `render()` with JSDOM
  - can manipulate or select elements using `screen` from rtl

- important part of testing is finding the elements that our component has created
  - test form submission
    - must find the button to click
  - test navigation
    - find a link to click
  - make sure element is visible
    - need to find the element
- system to find elements is tedious to use
- we use query functions to find elements we have rendered
- React Testing Library Query System provides approx 48 query functions
  - getByRole()
  - findAllByDisplayValue()
  - queryAllByRole()
  - queryByRole()
  - findByRole()
  - queryByLabelText()
  - findAllByTitle()
  - findByTitle()
  - getByLabelText()
- the names of the functions have a lot of meaning in terms of which to use when

## 13. Understanding ARIA roles

- getAllByRole and getByRole
- role refers too ARIA roles
  - they clarify the purpose of html element
  - traditionally used by screen readers
  - many html elements have implicit/automatically assigned roles
  - elements can be manually assigned a role
  - implicit assignment:
    <img src="./www.udemy.com_course_react-testing-library-and-jest_learn_lecture_35701626.png" alt="a diagram of some implicit ARIA roles and corresponding html elements">
- this ARIA role system is the preferred way of finding elements that have been rendered by our components
  - RTL pushes you in this direction
  - this means we need to understand ARIA role system

## 14. Understanding Jest matchers

- this refers to assertions which are our `expect()` functions
- expect is provided by Jest
- matchers are typically chained on our expect assertion like so:

```js
expect(inputs).toHaveLength(2);
```

- `toHaveLength` is a matcher
- there are Jest matchers and RTL matchers
- some Jest matchers
  - toHaveLength
  - toEqual
  - toContain
  - toThrow
  - toHaveBeenCalled
- some RTL matchers
  - toBeInTheDocument
  - toBeEnabled
  - toHaveClass
  - toHaveTextContent
  - toHaveValue
- Jest matchers are general purpose, can test any javascript
  - full list of jest matchers [here](https://jestjs.io/docs/expect "list of jest expect matchers")
- RTL matchers
  - jest-dom provides these extra matchers
  - more concerned with DOM elements
    - is present
    - has text, has attribute, etc
- a full list of RTL jest-dom matchers is available [here](https://github.com/testing-library/jest-dom#custom-matchers "a list of jest-dom custom matchers on github")

## 15. Test was not wrapped in act(...) warning and test failure

- a quick note about warnings related to act(...). This relates to RTL v14 update and CRA not updating its version of RTL.
- user events are now async
  - just have to make the test callback async and use await a bunch

## 16. Simulating User Events

- we're testing we can enter a name and email and click submit and then the onUserAdd function gets called
- we'll write a bad implementation of the test that works and fix it later
  - there are many different ways to write tests
  - one way might be better than another way
- to simulate clicks and keyboard entry we use the `user` object from RTL

```js
user.click(element);
user.keyboard("asdf");
user.keyboard("{Enter}");
```

## 17. Recording function calls

- it's easier to interpret results in the terminal if we just run one test file at a time sometimes
- run a single test in Jest:
- you can `npm i -g jest-cli` and then run `jest Component.test.js`
- if we run our tests, we'll get an error that states that onUserAdd is not defined
  - remember, this is the prop being passed to UserForm from App.jsx
  - since we are testing UserForm standalone, there is no prop or App.jsx for that matter!
- one way to fix this is to add an empty arrow function as follows:

```js
render(<UserForm onUserAdd={() => {}} />);
```

- the purpose of this test is to make sure it gets called, and called with the correct name and email, so this approach makes the error go away but isn't a good test
- how can we do this (badly for now)?
  - above our `render()` we define a new function and declare an array

```js
const argList = [];
const callBack = (...args) => {
  argList.push(args);
};
```

- now, when we submit our form, we can use argList to verify that our function was called and it was called with appropriate arguments
- like so:

```js
expect(argList).toHaveLength(1); // our callback got called at least one time
expect(argList[0][0]).toEqual({ name: "gavan", email: "gavan@email.com" });
```

- running our test we do actually get the `act(...)` warning
  - must update to v14 of rtl
- running our tests again, we get:

```js
 PASS  src/components/UserForm.test.js
  √ it shows two inputs and a button (131 ms)
  √ it calls onUserAdd when form is submitted (544 ms)

Test Suites: 1 passed, 1 total
Tests:       2 passed, 2 total
Snapshots:   0 total
Time:        3.306 s, estimated 4 s
Ran all test suites matching /userform.test.js/i.
```

- our tests are working, but it's not the best implementation and we'll address this later
- our UserForm.test.js so far:

```js
import { render, screen } from "@testing-library/react";
import user from "@testing-library/user-event";
import UserForm from "./UserForm";

test("it shows two inputs and a button", async () => {
  // render the component
  render(<UserForm />);

  // manipulate the component or find an element in it
  const inputs = screen.getAllByRole("textbox");
  const button = screen.getByRole("button");

  // Assertion - make sure component is doing what we expect it to do
  expect(inputs).toHaveLength(2);
  expect(button).toBeInTheDocument();
});

test("it calls onUserAdd when form is submitted", async () => {
  // not the best implementation
  const argList = [];
  const callBack = (...args) => {
    argList.push(args);
  };
  // try to render my component
  render(<UserForm onUserAdd={callBack} />);

  // find the two inputs
  const [nameInput, emailInput] = screen.getAllByRole("textbox");

  // simulate typing in a name
  await user.click(nameInput);
  await user.keyboard("gavan");

  // simulate typing in an email
  await user.click(emailInput);
  await user.keyboard("gavan@email.com");

  // find the button
  const button = screen.getByRole("button");

  // simulate clicking the button to submit the form
  user.click(button);

  // assertion to make sure 'onUserAdd' gets called with email and name
});
```

## 18 Introducing Mock Function

- we're fixing up our previous test implementation here
- argList and the callback are a hacky way to test that our function is called and called with correct args
- Jest has a tool to check just this scenario (function is called with correct args)
  - mock functions
- mock function
  - fake function that doesn't do anything
  - records when it gets called and the args it was called with
  - used very often when we need to make sure a component calls a callback
- steps
  - we create a mock function
  - we pass it down as onUserAdd prop to UserForm
  - mock func will have internal storage
    - will record how many times called
    - different args it receives when called
    - we'll simulate form submission
    - our mock func will get called
      - mock func will store arg it received
      - increase counter to record how many times called
      - we can write assertions about number of times called and args etc
- delete arg list and callback
- create a mock function like so:

```js
const mock = jest.fn();
```

- mock is now a function
- we pass it down to our component:

```js
render(<UserForm onUserAdd={mock} />);
```

- jest has builtin matchers for checking mock functions

```js
expect(mock).toHaveBeenCalled();
expect(mock).toHaveBeenCalledWith({ name: "gavan", email: "gavan@email.com" });
```

- running our tests, all pass
  - we had to stick await in front of our `user.click(button)`
- side note
  - installed nvm to let us update node and npm from the command line going forward

## 19. Querying elements by labels

- the next issue is:

```js
// find the two inputs
const [nameInput, emailInput] = screen.getAllByRole("textbox");
```

- we're assuming there will be only two inputs
  - we might reorder the inputs, in which case nameInput will reference the email input in the DOM
  - we might add or remove inputs
- any changes to our UserForm might make our tests begin to fail
  - this test is brittle - easy to break
    - changes will cause the test to fail even though the component may be working correctly
- a better way
  - need to understand forms, labels and inputs better
  - basic html stuff
    - if a label's 'for' attribute matches an input's 'id,' clicking on the label will focus the input
- if we make sure our label has a 'htmlFor' and the input has a matching id then we can:

```js
screen.getByLabelText(/enter email/i);
screen.getByRole("textbox", { name: /enter email/i });
```

- getByRole is using a filter object with regular expression to match the label text
- rtl prefers or recommends roles
- updating our test:

```js
// find the two inputs
const nameInput = screen.getByRole("textbox", { name: /enter name/i });
const emailInput = screen.getByRole("textbox", { name: /enter email/i });
```

- more flexible
- we can change order or add more inputs and tests won't break

## 20. Testing UserList

- receives array of users each with name and email
- renders table row, 2 data cells for every user
- important parts of this component?
  - shows one line per user
  - shows correct name and email for each user
- our code so far

```js
import { render, screen } from "@testing-library/react";
import UserList from "./UserList";

test("render one row per user", async () => {
  // render the component
  const users = [
    { name: "gavan", email: "gavan@email.com" },
    { name: "sam", email: "sam@email.com" },
  ];
  render(<UserList users={users} />);
  // find the rows

  // assertion: correct number of rows in the table
});

test("render the name and email of each user", async () => {
  render(<UserList />);
});
```

- how do we work out what the best way to find our rows is?

## 21. Getting Help with query functions

- memorizing query functions to find elements is hard
- can use this helper function:

```js
screen.logTestingPlaygroundURL();
```

- this creates html rendered by your component and creates a link to view the html in the testing playground tool
  - helps you to write queries - functions to find elements
- adding this to our test file and running we get:

```js
https://testing-playground.com/#markup=DwEwlgbgfMAuCGAjANgUwAQGNnwM64F4AiXTAWkRADsBPAMQAt0ArAVwEUAFAaQGEiYsBqnghBAJ0EMoAOXgBbVMAD0QqVACi8+GGQq1+yfuGjBiAPYgaEwWIDm8CPCr6xce4+cABVNt0A6THN5VxhVI1gIsVwFUPcoGPkfP2RA4LjwsNgLKyykNDDwaCA
```

- the long string represents html generated by component in enccoded format
- Testing Playground notes
  - terminal on left has querySelector code that will probably work to select ouru element 100% of the time
  - suggested query on right is green or blue and it may or may not be possible to use it directly as suggested
  - another downside, we are trying to find the rows but we can't click on the row, only the cells
    - can add custom styling to the html directly in top-left panel
    - add padding, and will be easier to click
- add this inline to the element you want to click on and find

```js
style = "border: 10px solid red; display: block;";
```

- our find and assertion:

```js
const rows = screen.getAllByRole("row");

// assertion: correct number of rows in the table
expect(rows).toHaveLength(2);
```

- however, there's a header row, so we fail the test

## 22 Query Function escape hatches

-
