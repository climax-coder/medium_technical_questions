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