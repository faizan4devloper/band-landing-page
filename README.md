import React, { useState } from "react";
import { useNavigate } from "react-router-dom";
import styles from "./LoginPage.module.css";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faEnvelope, faLock } from "@fortawesome/free-solid-svg-icons";
import backgroundImage from "../assets/LoginImg.jpg"; // Path to image

const LoginPage = () => {
  const [isLogin, setIsLogin] = useState(true);
  const navigate = useNavigate();

  const toggleForm = () => {
    setIsLogin(!isLogin);
  };

  const handleNavigate = () => {
    navigate("/personas"); // Navigate to the personas page
  };

  return (
    <div className={styles.container}>
      <div
        className={styles.leftSide}
        style={{ backgroundImage: `url(${backgroundImage})` }}
      >
        <div className={styles.overlay}></div>
        <div className={styles.overlayText}>Citizen Advisor</div>
      </div>

      <div className={styles.rightSide}>
        <div className={styles.formContainer}>
          <h2 className={styles.title}>{isLogin ? "Login" : "Register"}</h2>

          <form>
            <div className={styles.inputGroup}>
              <FontAwesomeIcon icon={faEnvelope} className={styles.icon} />
              <input
                type="email"
                className={styles.input}
                placeholder="Enter your email"
              />
            </div>

            <div className={styles.inputGroup}>
              <FontAwesomeIcon icon={faLock} className={styles.icon} />
              <input
                type="password"
                className={styles.input}
                placeholder="Enter your password"
              />
            </div>

            <button className={styles.button}>
              {isLogin ? "Login" : "Sign Up"}
            </button>
          </form>

          <div className={styles.toggleText} onClick={toggleForm}>
            {isLogin ? "Create an Account" : "Already have an account? Login"}
          </div>

          {/* Add Button to navigate to Personas */}
          <button className={styles.personasButton} onClick={handleNavigate}>
            Go to Personas
          </button>
        </div>
      </div>
    </div>
  );
};

export default LoginPage;







/* LoginPage.module.css */
.container {
  display: flex;
  height: 100vh;
}

.leftSide {
  display: none;
  justify-content: center;
  align-items: center;
  width: 50%;
  position: relative;
  background-size: cover;
  background-position: center;
}

.overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
}

.overlayText {
  position: relative;
  z-index: 2;
  color: white;
  font-size: 2rem;
  font-weight: bold;
  text-align: center;
  background: rgba(0, 0, 0, 0.5);
  padding: 1.5rem;
  border-radius: 0.5rem;
  animation: fadeIn 1.5s ease-in-out;
}

@keyframes fadeIn {
  0% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}

.rightSide {
  display: flex;
  flex-direction: column;
  justify-content: center;
  width: 50%;
  padding: 2rem;
  background-color: white;
  box-shadow: 0 10px 15px rgba(0, 0, 0, 0.1);
  animation: slideIn 1.5s ease-in-out;
}

@keyframes slideIn {
  0% {
    transform: translateX(100%);
  }
  100% {
    transform: translateX(0);
  }
}

.formContainer {
  max-width: 400px;
  margin: 0 auto;
}

.button {
  width: 100%;
  padding: 0.75rem;
  background-color: #4f46e5;
  color: white;
  font-weight: bold;
  border-radius: 0.375rem;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.button:hover {
  background-color: #4338ca;
}

.personasButton {
  margin-top: 1rem;
  padding: 0.75rem;
  width: 100%;
  background-color: #4caf50;
  color: white;
  font-weight: bold;
  border-radius: 0.375rem;
  cursor: pointer;
  transition: background-color 0.3s ease;
  animation: bounce 1.2s infinite;
}

.personasButton:hover {
  background-color: #388e3c;
}

/* Bounce animation */
@keyframes bounce {
  0%, 20%, 50%, 80%, 100% {
    transform: translateY(0);
  }
  40% {
    transform: translateY(-10px);
  }
  60% {
    transform: translateY(-5px);
  }
}

.toggleText {
  margin-top: 1rem;
  text-align: center;
  color: #4f46e5;
  font-weight: bold;
  cursor: pointer;
}

@media (min-width: 768px) {
  .leftSide {
    display: flex;
  }
}
