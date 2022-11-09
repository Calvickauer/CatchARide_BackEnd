# Catch A Ride (United States)

Catch-A-Ride is an American online marketplace for carpooling. Our website connects drivers and passengers willing to travel together between cities at an agreed upon price.


# Catch A Ride Frontend

| Components | Links to Code | Description |
| --- | --- | --- |
| `App`| [`App`](https://github.com/calvickauer/catch-a-ride-frontend#app-component) | The component that manages the entire app |
| `Signup`| [`Signup`](https://github.com/calvickauer/catch-a-ride-frontend/blob/main/docs/signup.md) | Allow the user to signup |
| `Login`| [`Login`](https://github.com/calvickauer/catch-a-ride-frontend/blob/main/docs/login.md) | Allows the user to login to the app |
| `Navbar`| [`Navbar`](https://github.com/calvickauer/catch-a-ride-frontend/blob/main/docs/navbar.md) | Make `App` class component |
| `Profile`| [`Profile`](#) | A component that displays the user profile information |
| `setAuthToken`| [`setAuthToken`](https://github.com/romebell/catch-a-ride-frontend/blob/main/docs/setAuthToken.md) | A utility function that adds a token to the `Authentication` header to manage current user |
| `About`| [`About`](https://github.com/romebell/catch-a-ride-frontend/blob/main/docs/other-components.md#about) | A component that decribes the app |
| `Footer`| [`Footer`](https://github.com/romebell/catch-a-ride-frontend/blob/main/docs/other-components.md#footer) | A footer that goes on each component |
| `Welcome`| [`Welcome`](https://github.com/romebell/catch-a-ride-frontend/blob/main/docs/other-components.md#welcome) | A welcome page for the user |

### `App Component`

### Imports for `App`

```jsx
// Imports
import React, { useEffect, useState } from 'react';
import { Route, Switch, Redirect, BrowserRouter as Router } from 'react-router-dom';
import jwt_decode from 'jwt-decode';
import setAuthToken from './utils/setAuthToken';

// CSS
import './App.css';

// Components
import Signup from './components/Signup';
import About from './components/About';
import Footer from './components/Footer';
import Login from './components/Login';
import Navbar from './components/Navbar';
import Profile from './components/Profile';
import VisitProfile from './components/VisitProfile';
import GoogleApiWrapper from './components/Welcome';
import Vehicle from './components/VehicleForm';
import JourneyOrRideForm from './components/JourneyOrRideReq';
import JourneyDetails from './components/JourneyDetails';
import Message from './components/MessageForm';
import VehicleEdit from './components/VehicleEdit';
import JourneyEdit from './components/JourneyEdit';
import Inbox from './components/Inbox';
import ProfileEdit from './components/ProfileEdit';
import Notfound from './components/404';
```

### `useState` inside `App.js`

```jsx
function App() {
  // Set state values
  const [currentUser, setCurrentUser] = useState('');
  const [isAuthenticated, setIsAuthenticated] = useState(true);
}
```

### `useState` inside `Image.js`

```jsx
// State Values
 const [state, setState] = useState({
    profileImg: ''
 })
```

### `PrivateRoute`

```jsx
const PrivateRoute = ({ component: Component, ...rest}) => {
  let token = localStorage.getItem('jwtToken');
  console.log('===> Hitting a Private Route');
  return <Route {...rest} render={(props) => {
    return token ? <Component {...rest} {...props} /> : <Redirect to="/login"/>
  }} />
}
```

### `useEffect` inside `App`

```jsx
useEffect(() => {
    let token;

    if (!localStorage.getItem('jwtToken')) {
      setIsAuthenticated(false);
      console.log('====> Authenticated is now FALSE');
    } else {
      token = jwt_decode(localStorage.getItem('jwtToken'));
      setAuthToken(localStorage.getItem('jwtToken'));
      setCurrentUser(token);
    }
  }, []);
```

### `nowCurrentUser`

```jsx
const nowCurrentUser = (userData) => {
    console.log('===> nowCurrent is here.');
    setCurrentUser(userData);
    setIsAuthenticated(true);
}
```

### `handleLogout`

```jsx
const handleLogout = () => {
    if (localStorage.getItem('jwtToken')) {
        // remove token for localStorage
        localStorage.removeItem('jwtToken');
        setCurrentUser(null);
        setIsAuthenticated(false);
    }
}
```

### `return` of `App`

```jsx
return (
<div className="App">
    <h1>MERN Authentication</h1>
    <Navbar handleLogout={handleLogout} isAuth={isAuthenticated} />
    <div className="container mt-5">
        <Switch>
            <Route path='/signup' component={Signup} />
            <Route 
            path="/login"
            render={(props) => <Login {...props} nowCurrentUser={nowCurrentUser} setIsAuthenticated={setIsAuthenticated} user={currentUser}/>}
            />
            <PrivateRoute path="/profile" component={Profile} user={currentUser} handleLogout={handleLogout} />
            <Route exact path="/" component={Welcome} />
            <Route path="/about" component={About} />
        </Switch>
    </div>
    <Footer />
</div>
);
```

### Finished

```jsx
// Imports
import React, { useEffect, useState } from 'react';
import { Route, Switch, Redirect } from 'react-router-dom';
import jwt_decode from 'jwt-decode';
import setAuthToken from './utils/setAuthToken';

// CSS
import './App.css';

// Components
import Signup from './components/Signup';
import About from './components/About';
import Footer from './components/Footer';
import Login from './components/Login';
import Navbar from './components/Navbar';
import Profile from './components/Profile';
import Welcome from './components/Welcome';

const PrivateRoute = ({ component: Component, ...rest}) => {
  let token = localStorage.getItem('jwtToken');
  console.log('===> Hitting a Private Route');
  return <Route {...rest} render={(props) => {
    return token ? <Component {...rest} {...props} /> : <Redirect to="/login"/>
  }} />
}

function App() {
  // Set state values
  const [currentUser, setCurrentUser] = useState('');
  const [isAuthenticated, setIsAuthenticated] = useState(true);

 
  useEffect(() => {
    let token;

    if (!localStorage.getItem('jwtToken')) {
      setIsAuthenticated(false);
      console.log('====> Authenticated is now FALSE');
    } else {
      token = jwt_decode(localStorage.getItem('jwtToken'));
      setAuthToken(localStorage.getItem('jwtToken'));
      setCurrentUser(token);
    }
  }, []);

  const nowCurrentUser = (userData) => {
    console.log('===> nowCurrent is here.');
    setCurrentUser(userData);
    setIsAuthenticated(true);
  }

  const handleLogout = () => {
    if (localStorage.getItem('jwtToken')) {
      // remove token for localStorage
      localStorage.removeItem('jwtToken');
      setCurrentUser(null);
      setIsAuthenticated(false);
    }
  }

  return (
    <div className="App">
      <Navbar handleLogout={handleLogout} isAuth={isAuthenticated} />
      <div className="container mt-5">
        <Switch>
          <Route path='/signup' component={Signup} />
          <Route 
            path="/login"
            render={(props) => <Login {...props} nowCurrentUser={nowCurrentUser} setIsAuthenticated={setIsAuthenticated} user={currentUser}/>}
          />
          <PrivateRoute path="/profile" component={Profile} user={currentUser} handleLogout={handleLogout} />
          <Route exact path="/" component={Welcome} />
          <Route path="/about" component={About} />
        </Switch>
      </div>
      <Footer />
    </div>
  );
}

export default App;
```