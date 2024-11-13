/* Add a subtle zoom effect on the background */
.leftSide {
  display: none;
  justify-content: center;
  align-items: center;
  width: 50%;
  position: relative;
  background-size: cover;
  background-position: center;
  overflow: hidden;
  animation: backgroundZoom 15s ease-in-out infinite;
}

@keyframes backgroundZoom {
  0%, 100% {
    transform: scale(1);
  }
  50% {
    transform: scale(1.1); /* Slight zoom */
  }
}

/* Enhanced overlay text styling */
.overlayText {
  position: relative;
  z-index: 2;
  color: white;
  font-size: 2.5rem;
  font-weight: bold;
  padding: 2rem;
  text-align: center;
  background: rgba(0, 0, 0, 0.6); /* Darker overlay for better contrast */
  border-radius: 0.75rem;
  opacity: 0;
  transform: translateY(-20px);
  animation: fadeInSlide 1.5s ease-out forwards;
}

/* Staggered fade-in for description */
.leftSide p {
  position: relative;
  z-index: 2;
  color: #f0f0f0;
  text-align: center;
  font-size: 1.2rem;
  padding: 1.5rem;
  margin: 0;
  opacity: 0;
  transform: translateY(20px);
  animation: fadeInSlide 1.8s ease-out forwards;
  animation-delay: 0.5s; /* More delay for staggered effect */
}

/* Update button hover effect */
.button:hover {
  background-color: #4338ca;
  transform: translateY(-3px) scale(1.02); /* Slight scaling for emphasis */
  box-shadow: 0 5px 15px rgba(67, 56, 202, 0.4);
}

/* Animation keyframes for fade-in and slide-up */
@keyframes fadeInSlide {
  0% {
    opacity: 0;
    transform: translateY(20px);
  }
  100% {
    opacity: 1;
    transform: translateY(0);
  }
}

@media (min-width: 768px) {
  .leftSide {
    display: flex;
  }
}







/* Container for the page layout */
.container {
  display: flex;
  height: 100vh;
}

/* Left side background with overlay */
.leftSide {
  display: none;
  justify-content: center;
  align-items: center;
  width: 50%;
  position: relative;
  background-size: cover;
  background-position: center;
  overflow: hidden;
}

.overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.6);
  z-index: 1;
  animation: overlayFadeIn 1s ease forwards;
}

@keyframes overlayFadeIn {
  from { opacity: 0; }
  to { opacity: 0.6; }
}

.overlayText {
  position: relative;
  z-index: 2;
  color: white;
  font-size: 2.5rem;
  font-weight: bold;
  padding: 2rem;
  text-align: center;
  background: rgba(0, 0, 0, 0.6);
  border-radius: 0.75rem;
  opacity: 0;
  transform: translateY(-20px);
  animation: fadeInSlide 1s ease-out forwards;
}

/* Fade-in for description */
.leftSide p {
  color: #f0f0f0;
  font-size: 1.2rem;
  opacity: 0;
  transform: translateY(20px);
  animation: fadeInSlide 1.2s ease-out forwards;
  animation-delay: 0.3s;
}

/* Right side with form */
.rightSide {
  display: flex;
  flex-direction: column;
  justify-content: center;
  width: 50%;
  background: linear-gradient(135deg, #FFDEE9 0%, #B5FFFC 100%);
  box-shadow: inset 0 0 15px rgba(0, 0, 0, 0.1);
}

.formContainer {
  max-width: 400px;
  margin: 0 auto;
  animation: fadeInForm 0.8s ease-out forwards;
}

@keyframes fadeInForm {
  from { opacity: 0; transform: translateY(30px); }
  to { opacity: 1; transform: translateY(0); }
}

.title {
  font-size: 2rem;
  font-weight: bold;
  text-align: center;
  color: #4f46e5;
  margin-bottom: 2rem;
}

/* Input fields and button styles */
.inputGroup {
  display: flex;
  align-items: center;
  background-color: #f3f4f6;
  padding: 0.75rem;
  border-radius: 0.375rem;
  margin-bottom: 1.5rem;
  border: 1px solid #e2e8f0;
  opacity: 0;
  transform: translateX(-20px);
  animation: fadeInInput 0.8s ease-out forwards;
}

@keyframes fadeInInput {
  from { opacity: 0; transform: translateX(-20px); }
  to { opacity: 1; transform: translateX(0); }
}

.icon {
  margin-right: 0.75rem;
  color: #4f46e5;
  font-size: 1rem;
}

.input {
  width: 100%;
  padding: 0.55rem;
  border: none;
  background: transparent;
  outline: none;
  font-size: 1rem;
}

/* Button with hover and pulse effect */
.button {
  width: 100%;
  padding: 0.85rem;
  background: linear-gradient(90deg, rgb(95, 30, 193) 0%, rgb(15, 95, 220) 100%);
  color: white;
  font-weight: bold;
  border: none;
  border-radius: 0.375rem;
  cursor: pointer;
  transition: transform 0.3s ease;
}

.button:hover {
  transform: scale(1.05);
  animation: pulse 1s infinite;
}

@keyframes pulse {
  0% { transform: scale(1); }
  50% { transform: scale(1.05); }
  100% { transform: scale(1); }
}

@media (min-width: 768px) {
  .leftSide {
    display: flex;
  }
}












import React, { useState } from "react";
import { useNavigate } from "react-router-dom";
import { useDispatch } from 'react-redux'; // For Redux
import { useAuthContext } from '../../context/AuthContext'; // For Context API
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
    if (email === "admin" && password === "1111") {
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
        <p>An experiance transformation from disconnected silos information to an intuitive. personalized revelations</p>
        </div>
      </div>

      <div className={styles.rightSide}>
        <div className={styles.formContainer}>
          <h2 className={styles.title}>Login</h2>

          <form onSubmit={handleLogin}>
            <div className={styles.inputGroup}>
              <FontAwesomeIcon icon={faEnvelope} className={styles.icon} />
              <input
                type="text"
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




/* Container for the page layout */
.container {
  display: flex;
  height: 100vh;
}

/* Left side background with overlay */
.leftSide {
  display: none;
  justify-content: center;
  align-items: center;
  width: 50%;
  position: relative;
  background-size: cover;
  background-position: center;
  overflow: hidden; /* Hide any overflow from animations */
}

.overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
  z-index: 1;
}

/* Fade-in and slide-in effect for headline text */
.overlayText {
  position: relative;
  z-index: 2;
  color: white;
  font-size: 2.5rem;
  font-weight: bold;
  padding: 2rem;
  text-align: center;
  background: rgba(0, 0, 0, 0.5);
  border-radius: 0.75rem;
  opacity: 0; /* Initially hidden */
  transform: translateY(-20px); /* Start a little above */
  animation: fadeInSlide 1s ease-out forwards;
}

/* Fade-in for description */
.leftSide p {
  position: relative;
  z-index: 2;
  color: #f0f0f0;
  text-align: center;
  font-size: 1.2rem;
  font-weight: normal;
  padding: 1.5rem;
  margin: 0;
  opacity: 0; /* Initially hidden */
  transform: translateY(20px); /* Start a little below */
  animation: fadeInSlide 1.2s ease-out forwards;
  animation-delay: 0.3s; /* Delay the description animation slightly */
}

/* Right side with form */
.rightSide {
  display: flex;
  flex-direction: column;
  justify-content: center;
  width: 50%;
  /*padding: 3rem;*/
  /*background-color: transparent;*/
background: linear-gradient(135deg, #FFDEE9 0%, #B5FFFC 100%);



  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.15);
  /*border-radius: 1rem;*/
}

.formContainer {
  max-width: 400px;
  margin: 0 auto;
}

.title {
  font-size: 2rem;
  font-weight: bold;
  text-align: center;
  color: #4f46e5;
  margin-bottom: 2rem;
}

/* Input fields and button styles */
.inputGroup {
  display: flex;
  align-items: center;
  background-color: #f3f4f6;
  padding: 0.75rem;
  border-radius: 0.375rem;
  margin-bottom: 1.5rem;
  border: 1px solid #e2e8f0;
}

.icon {
  margin-right: 0.75rem;
  color: #4f46e5;
  font-size: 1rem;
}

.input {
  width: 100%;
  padding: 0.55rem;
  border: none;
  background: transparent;
  outline: none;
  font-size: 1rem;
}

/* Button with hover effect */
.button {
  width: 100%;
  padding: 0.85rem;
  background: linear-gradient(90deg, rgb(95, 30, 193) 0%, rgb(15, 95, 220) 100%);
  color: white;
  font-weight: bold;
  border: none;
  border-radius: 0.375rem;
  cursor: pointer;
  transition: background-color 0.3s ease, transform 0.3s ease;
}

.button:hover {
  background-color: #4338ca;
  transform: translateY(-2px);
}

/* Animation keyframes for fade-in and slide-up */
@keyframes fadeInSlide {
  0% {
    opacity: 0;
    transform: translateY(20px);
  }
  100% {
    opacity: 1;
    transform: translateY(0);
  }
}

@media (min-width: 768px) {
  .leftSide {
    display: flex;
  }
}
