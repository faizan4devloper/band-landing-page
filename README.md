import React, { useState } from "react";
import styles from "./Login.module.css"; // Import the CSS module

const Login = () => {
  const [isLogin, setIsLogin] = useState(true);

  const toggleForm = () => {
    setIsLogin(!isLogin);
  };

  return (
    <div className={styles.container}>
      {/* Left side - Image */}
      <div className={styles.leftSide}>
        <div className={styles.overlayText}>
          Welcome Back!
        </div>
      </div>

      {/* Right side - Login form */}
      <div className={styles.rightSide}>
        <div className={styles.formContainer}>
          <h2 className={styles.title}>
            {isLogin ? "Login" : "Register"}
          </h2>

          <form>
            <div>
              <label className={styles.label} htmlFor="email">
                Email
              </label>
              <input
                id="email"
                type="email"
                className={styles.input}
                placeholder="Enter your email"
              />
            </div>

            <div>
              <label className={styles.label} htmlFor="password">
                Password
              </label>
              <input
                id="password"
                type="password"
                className={styles.input}
                placeholder="Enter your password"
              />
            </div>

            <div>
              <button
                type="button"
                className={styles.button}
              >
                {isLogin ? "Login" : "Sign Up"}
              </button>
            </div>
          </form>

          <div className={styles.toggleText} onClick={toggleForm}>
            {isLogin ? "Create an Account" : "Already have an account? Login"}
          </div>
        </div>
      </div>
    </div>
  );
};

export default Login;



/* Login.module.css */

.container {
  display: flex;
  height: 100vh;
  background: linear-gradient(to right, #4f46e5, #7c3aed);
}

.leftSide {
  display: none;
  justify-content: center;
  align-items: center;
  width: 50%;
  background-image: url('https://source.unsplash.com/random/800x800?tech');
  background-size: cover;
  background-position: center;
}

.overlayText {
  color: white;
  font-size: 2rem;
  font-weight: bold;
  padding: 1.5rem;
  text-align: center;
  background: rgba(0, 0, 0, 0.5);
  border-radius: 0.5rem;
}

.rightSide {
  display: flex;
  flex-direction: column;
  justify-content: center;
  width: 100%;
  padding: 2rem;
  background-color: white;
  border-radius: 0.5rem;
  box-shadow: 0 10px 15px rgba(0, 0, 0, 0.1);
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
  margin-bottom: 1.5rem;
}

.label {
  font-weight: bold;
  color: #4a5568;
  margin-bottom: 0.5rem;
}

.input {
  width: 100%;
  padding: 0.75rem;
  border: 1px solid #e2e8f0;
  border-radius: 0.375rem;
  margin-bottom: 1.5rem;
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
