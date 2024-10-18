const initialState = {
  isAuthenticated: false,
};

export const authReducer = (state = initialState, action) => {
  switch (action.type) {
    case 'LOGIN':
      return { ...state, isAuthenticated: true };
    case 'LOGOUT':
      return { ...state, isAuthenticated: false };
    default:
      return state;
  }
};



import { createStore, combineReducers } from 'redux';
import { authReducer } from './authReducer'; // Import your authReducer

const rootReducer = combineReducers({
  auth: authReducer,
  // Add other reducers here if needed
});

const store = createStore(rootReducer);

export default store;




import React, { createContext, useReducer } from 'react';

const AuthContext = createContext();

const initialState = {
  user: null,
};

const authContextReducer = (state, action) => {
  switch (action.type) {
    case 'SET_USER':
      return { ...state, user: action.payload };
    case 'CLEAR_USER':
      return { ...state, user: null };
    default:
      return state;
  }
};

export const AuthProvider = ({ children }) => {
  const [state, dispatch] = useReducer(authContextReducer, initialState);

  return (
    <AuthContext.Provider value={{ state, dispatch }}>
      {children}
    </AuthContext.Provider>
  );
};

export const useAuthContext = () => React.useContext(AuthContext);







import React, { useState } from 'react';
import { BrowserRouter as Router, Route, Routes } from 'react-router-dom';
import { Provider, useDispatch, useSelector } from 'react-redux';
import Personas from './components/Personas/Personas';
import EducationPage from './components/Pages/Education/EducationPage';
import JobPage from './components/Pages/Job/JobPage';
import HealthPage from './components/Pages/Health/HealthPage';
import LoginPage from './components/Login/LoginPage';
import Header from './components/Header/Header';
import store from './redux/store'; // Import your Redux store
import { AuthProvider } from './context/AuthContext'; // Import Context Provider

function App() {
  // Use Redux state for authentication
  const isAuthenticated = useSelector((state) => state.auth.isAuthenticated);
  const dispatch = useDispatch(); // To dispatch actions

  return (
    <Provider store={store}>
      <AuthProvider>
        <Router>
          <div className="App">
            <Header />
            <Routes>
              <Route
                path="/"
                element={<LoginPage setIsAuthenticated={() => dispatch({ type: 'LOGIN' })} />}
              />
              {isAuthenticated ? (
                <>
                  <Route path="/personas" element={<Personas />} />
                  <Route path="/education" element={<EducationPage />} />
                  <Route path="/job" element={<JobPage />} />
                  <Route path="/health" element={<HealthPage />} />
                </>
              ) : (
                <Route path="*" element={<LoginPage setIsAuthenticated={() => dispatch({ type: 'LOGIN' })} />} />
              )}
            </Routes>
          </div>
        </Router>
      </AuthProvider>
    </Provider>
  );
}

export default App;





import React, { useState } from "react";
import { useNavigate } from "react-router-dom";
import { useDispatch } from 'react-redux'; // For Redux
import { useAuthContext } from '../context/AuthContext'; // For Context API
import styles from "./LoginPage.module.css";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faEnvelope, faLock } from "@fortawesome/free-solid-svg-icons";
import backgroundImage from "../assets/LoginImg.jpg";

const LoginPage = ({ setIsAuthenticated }) => {
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');
  const navigate = useNavigate();
  const dispatch = useDispatch(); // Redux dispatch
  const { dispatch: contextDispatch } = useAuthContext(); // Context API dispatch

  const handleLogin = (e) => {
    e.preventDefault();
    if (email === "user@example.com" && password === "password") {
      setIsAuthenticated(); // This triggers Redux login
      contextDispatch({ type: 'SET_USER', payload: { email } }); // Set user context
      navigate("/personas");
    } else {
      alert("Invalid credentials");
    }
  };

  return (
    <div className={styles.container}>
      <div
        className={styles.leftSide}
        style={{ backgroundImage: `url(${backgroundImage})` }}
      >
        <div className={styles.overlay}></div>
        <div className={styles.overlayText}>
          Citizen Advisor
        </div>
      </div>

      <div className={styles.rightSide}>
        <div className={styles.formContainer}>
          <h2 className={styles.title}>Login</h2>

          <form onSubmit={handleLogin}>
            <div className={styles.inputGroup}>
              <FontAwesomeIcon icon={faEnvelope} className={styles.icon} />
              <input
                type="email"
                className={styles.input}
                placeholder="Enter your email"
                value={email}
                onChange={(e) => setEmail(e.target.value)}
              />
            </div>

            <div className={styles.inputGroup}>
              <FontAwesomeIcon icon={faLock} className={styles.icon} />
              <input
                type="password"
                className={styles.input}
                placeholder="Enter your password"
                value={password}
                onChange={(e) => setPassword(e.target.value)}
              />
            </div>

            <button className={styles.button}>
              Login
            </button>
          </form>
        </div>
      </div>
    </div>
  );
};

export default LoginPage;
