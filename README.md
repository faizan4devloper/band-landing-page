import React, { useState } from "react";
import styles from "./LoginPage.module.css";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faEnvelope, faLock } from "@fortawesome/free-solid-svg-icons";
import backgroundImage from "../assets/LoginImg.jpg"; // Example path to image

const LoginPage = () => {
  const [isLogin, setIsLogin] = useState(true);

  const toggleForm = () => {
    setIsLogin(!isLogin);
  };

  return (
    <div className={styles.container}>
      {/* Left side with image and overlay */}
      <div
        className={styles.leftSide}
        style={{ backgroundImage: `url(${backgroundImage})` }}
      >
        <div className={styles.overlay}></div>
        <div className={styles.overlayText}>
          Welcome Back!
        </div>
      </div>

      {/* Right side login form */}
      <div className={styles.rightSide}>
        <div className={styles.formContainer}>
          <h2 className={styles.title}>{isLogin ? "Login" : "Register"}</h2>

          <form>
            {/* Email input with icon */}
            <div className={styles.inputGroup}>
              <FontAwesomeIcon icon={faEnvelope} className={styles.icon} />
              <input
                type="email"
                className={styles.input}
                placeholder="Enter your email"
              />
            </div>

            {/* Password input with icon */}
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
  position: relative; /* This is important for the overlay to position correctly */
  background-size: cover;
  background-position: center;
}

/* Overlay layer */
.overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5); /* Black color with 50% opacity */
  z-index: 1; /* Ensure the overlay stays above the image */
}

/* Text on top of the overlay */
.overlayText {
  position: relative; /* Ensures the text stays on top of the overlay */
  z-index: 2; /* Keep text on top of overlay */
  color: white;
  font-size: 2.5rem;
  font-weight: bold;
  padding: 2rem;
  text-align: center;
  background: rgba(0, 0, 0, 0.5);
  border-radius: 0.75rem;
}

.rightSide {
  display: flex;
  flex-direction: column;
  justify-content: center;
  width: 50%;
  padding: 3rem;
  background-color: white;
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.15);
  border-radius: 1rem;
}

.formContainer {
  max-width: 400px;
  margin: 0 auto;
}

.title {
  font-size: 2.5rem;
  font-weight: bold;
  text-align: center;
  color: #4f46e5;
  margin-bottom: 2rem;
}

/* Input group to contain input and icon */
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
  font-size: 1.2rem;
}

.input {
  width: 100%;
  padding: 0.75rem;
  border: none;
  background: transparent;
  outline: none;
  font-size: 1rem;
}

.button {
  width: 100%;
  padding: 0.85rem;
  background-color: #4f46e5;
  color: white;
  font-weight: bold;
  border-radius: 0.375rem;
  cursor: pointer;
  transition: background-color 0.3s ease, transform 0.3s ease;
}

.button:hover {
  background-color: #4338ca;
  transform: translateY(-2px);
}

.toggleText {
  margin-top: 1.5rem;
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

npm install @fortawesome/react-fontawesome @fortawesome/free-solid-svg-icons @fortawesome/fontawesome-svg-core
