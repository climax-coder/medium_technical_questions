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