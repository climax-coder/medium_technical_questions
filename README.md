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

