import React, { useState } from "react";
import styles from "./Login.module.css"; // Specific styles for this component
import backgroundImage from "../assets/background.jpg"; // Path to the local image

const Login = () => {
  const [isLogin, setIsLogin] = useState(true);

  const toggleForm = () => {
    setIsLogin(!isLogin);
  };

  return (
    <div className="flex h-screen">
      {/* Left Image Section with Gradient Overlay */}
      <div
        className={styles.leftImageContainer}
        style={{ backgroundImage: `url(${backgroundImage})` }}
      >
        <div className={styles.gradientOverlay}></div>
        <div className={`${styles.content} flex-center`}>
          {/* Content on top of the image */}
          <h1 className="text-white text-4xl font-bold">Welcome Back!</h1>
        </div>
      </div>

      {/* Right Form Section */}
      <div className={`${styles.loginForm} w-full flex-center`}>
        <div className={styles.formContainer}>
          <h2 className="text-2xl font-bold mb-4 text-primary">
            {isLogin ? "Login" : "Register"}
          </h2>

          <form>
            <input
              id="email"
              type="email"
              className="input-field mb-4"
              placeholder="Enter your email"
            />
            <input
              id="password"
              type="password"
              className="input-field mb-4"
              placeholder="Enter your password"
            />
            <button type="button" className="button-primary w-full">
              {isLogin ? "Login" : "Sign Up"}
            </button>
          </form>

          <button
            type="button"
            className="text-indigo-500 mt-4 hover:text-indigo-700"
            onClick={toggleForm}
          >
            {isLogin ? "Create an Account" : "Already have an account? Login"}
          </button>
        </div>
      </div>
    </div>
  );
};

export default Login;



/* Login.module.css */

.leftImageContainer {
  position: relative;
  width: 50%; /* Adjust this based on your layout preference */
  background-size: cover;
  background-position: center;
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100vh; /* Full height */
}

.gradientOverlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: linear-gradient(
    to bottom right,
    rgba(58, 34, 102, 0.7), /* Start color with opacity */
    rgba(56, 120, 235, 0.5)  /* End color with opacity */
  );
  z-index: 1;
}

.content {
  position: relative;
  z-index: 2;
  text-align: center;
}

.loginForm {
  width: 50%;
  padding: 2rem;
  display: flex;
  justify-content: center;
  align-items: center;
}

.formContainer {
  background-color: white;
  padding: 2rem;
  border-radius: 0.5rem;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
  width: 100%;
  max-width: 400px;
}
