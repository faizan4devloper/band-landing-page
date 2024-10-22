/* Left side container styling */
.leftSide {
  display: none;
  justify-content: center;
  align-items: center;
  width: 50%;
  position: relative;
  background-size: cover;
  background-position: center;
  overflow: hidden; /* Ensure content stays inside */
}

/* Overlay layer */
.overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.7); /* Darker opacity for more contrast */
  z-index: 1;
}

/* Highlighted Text */
.overlayText {
  position: relative;
  z-index: 2;
  color: white;
  font-size: 3rem;
  font-weight: bold;
  padding: 2rem;
  text-align: center;
  text-shadow: 0 0 10px rgba(255, 255, 255, 0.8), 0 0 20px rgba(255, 255, 255, 0.6);
  background: rgba(0, 0, 0, 0.5);
  border-radius: 0.75rem;
  animation: glow 2s ease-in-out infinite alternate;
}

/* Glowing effect for the highlighted text */
@keyframes glow {
  0% {
    text-shadow: 0 0 10px rgba(255, 255, 255, 0.6), 0 0 20px rgba(255, 255, 255, 0.4);
  }
  100% {
    text-shadow: 0 0 20px rgba(255, 255, 255, 1), 0 0 40px rgba(255, 255, 255, 0.8);
  }
}

/* Autocomplete effect for description text */
.descriptionText {
  position: relative;
  z-index: 2;
  color: #ddd;
  font-size: 1.2rem;
  line-height: 1.6;
  text-align: center;
  width: 80%;
  margin: 1rem auto;
  white-space: nowrap;
  overflow: hidden;
  border-right: 2px solid rgba(255, 255, 255, 0.75);
  box-sizing: border-box;
  animation: typing 4s steps(40, end), blink-caret 0.75s step-end infinite;
}

/* Typing effect */
@keyframes typing {
  from {
    width: 0;
  }
  to {
    width: 100%;
  }
}

/* Blinking cursor */
@keyframes blink-caret {
  50% {
    border-color: transparent;
  }
}

/* Responsive design for larger screens */
@media (min-width: 768px) {
  .leftSide {
    display: flex;
  }
}



import React, { useState } from "react";
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
  const navigate = useNavigate();
  const dispatch = useDispatch();
  const { dispatch: contextDispatch } = useAuthContext();

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
        <div className={styles.overlayText}>Citizen Advisor</div>
        <p className={styles.descriptionText}>
          An experience transformation from disconnected silos of information to intuitive, personalized revelations.
        </p>
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
