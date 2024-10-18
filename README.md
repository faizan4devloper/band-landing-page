import React, { useState } from "react";
import styles from "./LoginPage.module.css";
import backgroundImage from "../assets/LoginImg.jpg"; // Path to your image

const LoginPage = () => {
  const [isLogin, setIsLogin] = useState(true);

  const toggleForm = () => {
    setIsLogin(!isLogin);
  };

  return (
    <div
      className={styles.container}
      style={{ backgroundImage: `url(${backgroundImage})` }}
    >
      {/* The form container with an overlay */}
      <div className={styles.overlay}></div>
      <div className={styles.formContent}>
        <div className={styles.formContainer}>
          <h2 className={styles.title}>{isLogin ? "Login" : "Register"}</h2>

          <form>
            <div className={styles.inputGroup}>
              <input
                type="email"
                className={styles.input}
                placeholder="Enter your email"
              />
            </div>

            <div className={styles.inputGroup}>
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
