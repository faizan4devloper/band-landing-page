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
  const titleText = "Login";

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
          Citizen Advisor
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







/* Blinking cursor for auto-typing effect */
.cursor {
  display: inline-block;
  animation: blink 0.7s steps(1) infinite;
}

@keyframes blink {
  0%, 100% { opacity: 1; }
  50% { opacity: 0; }
}
