# Advanced React Patterns on Reuse: Render props with higher order functions.
import {useState, React, useEffect} from 'react'
import './App.css';

const compose = (...funcs) =>
  initialArg => funcs.reduce((acc, func) => func(acc), initialArg);

function addAge(userData) {
  const objCopy = Object.assign({}, userData)
  objCopy.age += 100
  return objCopy
}

function addLastName(userData) {
  const objCopy = Object.assign({}, userData)
  objCopy.name = `${objCopy.name} Doe`
  return objCopy
}

function present(userData) {
  return (
    <>
      <p>{userData.name}</p>
      <p>{userData.age}</p>
      <p>{userData.city}</p>
    </>
  )
}


function UserProfile({render, url}) {

  const [userData, setUserData] = useState({})

  useEffect(() => { 
    url.includes('employee') ?
    setUserData(
      {
        name: 'John',
        age: 44,
        city: 'Philadelphia, PA'
      }
    )
    :
    setUserData(
      {
        name: 'Jimmy',
        age: 39,
        city: 'New York, NY'
      }
    )
  }, []); 

  return render({userData});

}

function App() {
  return (
    <div className="App">
        <UserProfile url='employee' render={ ({userData}) => {
          return compose(addAge, addLastName, present)(userData)
        }} />
    <hr/>
    <UserProfile url='customer' render={ ({userData}) => {
          return compose(addAge, addLastName, present)(userData)
        }} />
    </div>
  );
}

export default App;


# React Interview Questions asked to me with answers.

## Can you provide an overview of your experience with React?

Answer: I have experience building web applications using React, including creating reusable components and managing application state with hooks and the Context API. I have also worked with popular libraries such as Redux and React Router.

## Can you explain the purpose of the ref attribute in the lifecycle of a functional component in React?

Answer: The ref attribute in React is used to store a reference to a DOM element or a class-based component instance. It can be used to access the properties of the element, such as its value or checked state, or to invoke methods on the element. In the lifecycle of a functional component, the ref attribute can be used to access the DOM element after it has been rendered.

## Use Infinitive Scroll in React Like a PRO

### Why do we need to use Infinitive Scroll?
When we fetch data from the server, usually we have to use pagination/infinite scroll. Because if we try to fetch all of the data, it can cause performance issues. We usually prefer infinitive scroll over pagination because of UX.

Infinite scrolling is a listing-page design approach that loads content continuously as the user scrolls down. It eliminates the need for pagination — breaking content up into multiple pages.

When we scroll down, we will check the scroll position and if scroll near to bottom we call the fetch function.
But when the user scrolls down fastly, it can execute this function more than once. 

### What is the throttle function?
Throttling will simply prevent a function from running if it has run recently, regardless of the call frequency. It works like debouncing.

Both debounce and throttle are techniques used to limit the frequency of function calls, especially when dealing with user events such as scrolling, resizing, or typing. The main difference between debounce and the throttle is how they control the frequency of function calls.

### Debouncing vs Throttle
Debouncing is a technique that ensures that a function is called only after a certain amount of time has passed since the last invocation. In other words, if a function is called repeatedly within a short amount of time, debounce will wait until the calls have stopped for a specified duration before invoking the function. Debouncing is useful for scenarios where you want to trigger an action only after a user has stopped a continuous activity, such as typing.

Throttling is a technique that limits the rate at which a function can be called. It ensures that a function is called no more than once every specified interval of time. In other words, if a function is called multiple times within the specified interval, the throttle will ignore all subsequent calls until the interval has elapsed, at which point it will invoke the function again. Throttling is useful for scenarios where you want to ensure that a function is called at a regular interval, such as when animating elements or handling network requests.

So, debounce delays the invocation of a function until a certain time has elapsed since the last call, while throttle limits the frequency of function calls to a specified interval.

I’m using lodash library to get the throttle function but also we can write our own throttling function.

The throttling function has 3 parameters, first one is a function to execute. The second one is time to wait for another call. And the third one is options. I wanted to add {trailing: false} , because I wanted to prevent executing code at the end. So that’s a better approach.

 const fetchData = throttle(async () => {
    try {
      const API = 'https://jsonplaceholder.typicode.com/posts?';

      const res = await fetch(API + new URLSearchParams({
        offset: data.length
      }));
      const resData = await res.json();
      setData([...data, ...resData])

    } catch (error) {
      console.log(error);
    } finally {
      setIsLoading(false);
    }
  }, 2000, { trailing: false });

## Chat application using stream-chat-react library

https://github.com/adrianhajdin/project_medical_pager_chat/blob/master/client/src/App.jsx
A chat application shows off my mastery of web sockets and real-time communication. It is a challenging coding project. Users should be able to send messages and enter various chat groups using the app. You can store the data in a database like MongoDB or Firebase and use a web framework like Socket.io or Pusher to do it.

## How to prevent re-renders in React

https://adevnadia.medium.com/react-re-renders-guide-preventing-unnecessary-re-renders-8a3d2acbdba3

## The Complete JavaScript Handbook
### Promise, Async, Await

https://medium.com/p/f26b2c71719c#cc01

## How Does the useDeferredValue Hook Work in React?

useDeferredValue in action
This is a situation where the useDeferredValue hook is handy. useDeferredValue() accepts a state value as an argument and returns a copy of the value that will be deferred, i.e., when the state value is updated, the copy will not update accordingly until after the DOM has been updated to reflect the state change. This ensures that urgent updates happen and are not delayed by less critical, time-consuming ones.

function App() {
  const [query, setQuery] = useState('');
  // 👇 useDefferedValue
  const deferredQuery = useDefferedValue(query);
  const list = useMemo(() => {
    return largeList.filter((item) => item.name.includes(query));
  }, [deferredQuery]);
  const handleChange = (event) => {
    setQuery(event.target.value);
  };
  return (
    <>
      <input type="text" value={query} onChange={handleChange} placeholder="Search" />
      {list.map((item) => (
        <SearchResultItem key={item.id} item={item} />
      ))}
    </>
  );
}

If we keep changing the input field’s text in a short period (e.g., by typing fast), the deferredQuery state will remain unchanged and the list will not be updated. This is because the query state will keep changing before useDeferredValue can be updated, so useDeferredValue will continue to delay the update until it has time to set deferredQuery to the latest value of query and update the list.

This is quite similar to debouncing, as the list is not updated till a while after input has stopped.

## React Props Cheatsheet: 10 Patterns You Should Know

https://medium.com/webdevhero/react-props-cheatsheet-10-patterns-you-should-know-8dde4b9410d8
#### 1. Props can be passed conditionally
#### 2. Props passed with just their name have a value of true
#### 3. Props can be accessed as an object or destructured
#### 4. Components can be passed as props (including children)
#### 5. Anything can be passed as a prop (especially functions)
#### 6. Update a prop’s value with state
#### 7. Props can be spread in individually
#### 8. Props can be given a default value if none is provided
#### 9. Props can be renamed to avoid errors
#### 10. Don’t attempt to destructure props multiple times

## Understanding Design Patterns in JavaScript

https://blog.bitsrc.io/understanding-design-patterns-in-javascript-13345223f2dd

#### What is a Design Pattern?
In software engineering, a design pattern is a reusable solution for commonly occurring problems in software design. Design patterns represent the best practices used by the experienced software developers. A design pattern can be thought of as a programming template.

#### Why use Design Patterns?
Many programmers either think design patterns are a waste of time or they don’t know how to apply them appropriately. But using an appropriate design pattern can help you to write better and more understandable code, and the code can be easily maintained because it’s easier to understand.

Most importantly, the design patterns give software developers a common vocabulary to talk about. They show the intent of your code instantly to someone learning the code.

For example, if you are using decorator pattern in your project, then a new programmer would immediately know what that piece of code is doing, and they can focus more on solving the business problem rather than trying to understand what that code is doing.

Now that we know what design patterns are, and why they are important, let’s dive into various design patterns used in JavaScript.

#### Module Pattern
A Module is a piece of self-contained code so we can update the Module without affecting the other parts of the code. Modules also allow us to avoid namespace pollution by creating a separate scope for our variables. We can also reuse modules in other projects when they are decoupled from other pieces of code.

Modules are an integral part of any modern JavaScript application and help in keeping our code clean, separated and organized. There are many ways to create modules in JavaScript, one of which is Module pattern.

#### Revealing Module Pattern
The Revealing Module pattern is a slightly improved version of the module pattern by Christian Heilmann. The problem with the module pattern is that we have to create new public functions just to call the private functions and variables.

#### ES6 Modules
Before ES6, JavaScript didn’t have built-in modules, so developers had to rely on third-party libraries or the module pattern to implement modules. But with ES6, JavaScript has native modules.

ES6 modules are stored in files. There can only be one module per file. Everything inside a module is private by default. Functions, variables, and classes are exposed using the export keyword. The code inside a module always runs in strict mode.

#### Singleton Pattern
A Singleton is an object which can only be instantiated only once. A singleton pattern creates a new instance of a class if one doesn’t exist. If an instance exists, it simply returns a reference to that object. Any repeated calls to the constructor would always fetch the same object.

#### Factory Pattern
Factory Pattern is a pattern that uses factory methods to create objects without specifying the exact class or constructor function from which the object will be created.

The factory pattern is used to create objects without exposing the instantiation logic. This pattern can be used when we need to generate a different object depending upon a specific condition.

#### Decorator Pattern
A Decorator pattern is used to extend the functionality of an object without modifying the existing class or constructor function. This pattern can be used to add features to an object without modifying the underlying code using them.