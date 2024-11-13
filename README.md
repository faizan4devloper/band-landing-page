import React, { useState, useEffect } from "react";
import { useNavigate } from "react-router-dom";
import { useDispatch } from 'react-redux';
import { useAuthContext } from '../../context/AuthContext';
import styles from "./LoginPage.module.css";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faEnvelope, faLock } from "@fortawesome/free-solid-svg-icons";
import backgroundImage from "../assets/LoginImg.jpg";

const LoginPage = ({ setIsAuthenticated }) => {
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');
  const [typedTitle, setTypedTitle] = useState(''); // State for typed title effect
  const navigate = useNavigate();
  const dispatch = useDispatch();
  const { dispatch: contextDispatch } = useAuthContext();
  const titleText = "Hello Again! Please Log In";

  useEffect(() => {
    let index = 0;
    const typingInterval = setInterval(() => {
      setTypedTitle(titleText.slice(0, index + 1));
      index++;
      if (index === titleText.length) {
        clearInterval(typingInterval);
      }
    }, 150); // Typing speed
    return () => clearInterval(typingInterval);
  }, []);

  const handleLogin = (e) => {
    e.preventDefault();
    if (email === "admin" && password === "1111") {
      setIsAuthenticated();
      contextDispatch({ type: 'SET_USER', payload: { email } });
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
         <span>Citizen Advisor</span>
          <p>An experience transformation from disconnected silos of information to intuitive, personalized revelations</p>
        </div>
      </div>

      <div className={styles.rightSide}>
        <div className={styles.formContainer}>
          <h2 className={styles.title}>{typedTitle}<span className={styles.cursor}>|</span></h2>

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
  padding: 4rem 2rem;
  text-align: center;
  background: rgba(0, 0, 0, 0.5);
  border-radius: 0.75rem;
  opacity: 0; /* Initially hidden */
  transform: translateY(-20px); /* Start a little above */
  animation: fadeInSlide 1s ease-out forwards;
  
  
  
}

.overlayText span{
    background: -webkit-gradient(linear, left top, right top, color-stop(-19.51%, #7abef7), color-stop(36.51%, #dfdfdf), to(#572ac2));
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  text-fill-color: transparent; /* For compatibility with non-Webkit browsers */
}

/* Fade-in for description */
.leftSide p {
  position: relative;
  z-index: 2;
  color: #f0f0f0;
  text-align: center;
  font-size: 1rem;
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
  font-size: 1.2rem;
  font-weight: bold;
  text-align: center;
  color: #4f46e5;
  margin-bottom: 2rem;
  
  
   background: -webkit-gradient(linear, left top, right top, 
    color-stop(-19.51%, #7abef7), 
    color-stop(36.51%, #4080f5), 
    to(#572ac2));
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  text-fill-color: transparent; /* For compatibility with non-Webkit browsers */
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


/* Blinking cursor for auto-typing effect */
.cursor {
  display: inline-block;
  animation: blink 0.7s steps(1) infinite;
}

@keyframes blink {
  0%, 100% { opacity: 1; }
  50% { opacity: 0; }
}
