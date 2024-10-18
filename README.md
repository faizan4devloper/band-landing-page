import './App.css';
import LoginPage from './components/Login/LoginPage';
import Header from './components/Header/Header';
import Dashboard from './components/Personas/Personas'; // New Dashboard component
import { BrowserRouter as Router, Route, Routes, Navigate } from 'react-router-dom';
import { useState } from 'react';

function App() {
  const [isAuthenticated, setIsAuthenticated] = useState(false); // Manage authentication state

  return (
    <Router>
      <div className="App">
        <Header />
        <Routes>
          {/* Route for the login page */}
          <Route 
            path="/login" 
            element={<LoginPage setIsAuthenticated={setIsAuthenticated} />} 
          />

          {/* Route for the dashboard (accessible only if authenticated) */}
          <Route 
            path="/personas" 
            element={isAuthenticated ? <Personas /> : <Navigate to="/login" />} 
          />

          {/* Default route */}
          <Route 
            path="/" 
            element={<Navigate to={isAuthenticated ? "/personas" : "/login"} />} 
          />
        </Routes>
      </div>
    </Router>
  );
}

export default App;





import React, { useState } from "react";
import { useNavigate } from "react-router-dom"; // To navigate after login
import styles from "./LoginPage.module.css";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faEnvelope, faLock } from "@fortawesome/free-solid-svg-icons";
import backgroundImage from "../assets/LoginImg.jpg"; 

const LoginPage = ({ setIsAuthenticated }) => {
  const [isLogin, setIsLogin] = useState(true);
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');
  const navigate = useNavigate(); // For redirecting the user

  const toggleForm = () => {
    setIsLogin(!isLogin);
  };

  const handleLogin = (e) => {
    e.preventDefault();
    // Placeholder for actual authentication logic
    if (email === "user@example.com" && password === "password") {
      setIsAuthenticated(true);
      navigate("/personas"); // Redirect to the dashboard
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
          <h2 className={styles.title}>{isLogin ? "Login" : "Register"}</h2>

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
              {isLogin ? "Login" : "Sign Up"}
            </button>
          </form>

          <div className={styles.toggleText} onClick={toggleForm}>
            {isLogin ? "Create an Account" : "Already have an account? Login"}
          </div>
        </div>
      </div>
    </div>
  );
};

export default LoginPage;




import React from 'react';
import styles from './Personas.module.css';

const Personas = () => {
  return (
    <div className={styles.Personas}>
      <h1>Welcome to the Dashboard</h1>
      <p>This is your post-login page. You can add more content here.</p>
    </div>
  );
};

export default Personas;



.Personas {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
  background-color: #f4f4f9;
}

h1 {
  font-size: 2.5rem;
  color: #333;
}

p {
  font-size: 1.2rem;
  color: #666;
}
