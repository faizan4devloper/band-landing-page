import React from "react";
import styles from "./Header.module.css";

import Logo from '../assets/HCLTechLogoBlue.svg';

const Header = () => {
  return (
    <header className={styles.header}>
      <div className={styles.logoContainer}>
        <span className={styles.logo}>Claim Assist</span>
      </div>
      
      <div className={styles.authButtons}>
        <img className={styles.rightLogo} src={Logo} alt="logo" />
      </div>
    </header>
  );
};

export default Header;



.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 14px 80px;
  background-color: #ffffff;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  position: relative;
  overflow: hidden;
}

.logoContainer {
  position: relative;
}

.logo {
  font-size: 1.8rem;
  font-weight: bold;
  color: rgb(15, 95, 220);
  position: relative;
  z-index: 1;
}

.logoContainer::before {
  content: "";
  position: absolute;
  left: 0;
  top: 50%;
  transform: translateY(-50%) translateX(-100%);
  width: 100%;
  height: 1px;
  background: linear-gradient(90deg, rgb(15, 95, 220), transparent);
  animation: slideIn 2s cubic-bezier(0.65, 0, 0.35, 1) infinite;
  z-index: 0;
}

@keyframes slideIn {
  0% {
    transform: translateY(-50%) translateX(-100%);
  }
  50% {
    transform: translateY(-50%) translateX(50%);
  }
  100% {
    transform: translateY(-50%) translateX(100%);
  }
}

.rightLogo {
  width: 90px;
  transform: scale(0.9);
  transition: transform 0.3s ease, opacity 0.3s ease;
}

.rightLogo:hover {
  transform: scale(1.1);
  opacity: 0.8;
}

.authButtons {
  display: flex;
  align-items: center;
}

.navbar a {
  text-decoration: none;
  color: #333;
  font-size: 1rem;
  transition: color 0.3s ease;
}

.navbar a:hover {
  color: #4a90e2;
}

.loginButton,
.signupButton {
  padding: 10px 20px;
  font-size: 0.9rem;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.loginButton {
  background-color: #ffffff;
  color: #4a90e2;
  border: 1px solid #4a90e2;
}

.loginButton:hover {
  background-color: #4a90e2;
  color: #ffffff;
}

.signupButton {
  background-color: #4a90e2;
  color: #ffffff;
}

.signupButton:hover {
  background-color: #357abd;
}
