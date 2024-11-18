import React from "react";
import styles from "./Header.module.css";

import Logo from '../assets/HCLTechLogoBlue.svg';

const Header = () => {
  return (
    <header className={styles.header}>
      <div className={styles.logo}>Claim Assist</div>
      
      <div className={styles.authButtons}>
       <img className={styles.rightlogo} src={Logo}  alt="logo"/>
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
}

.logo {
  font-size: 1.4rem;
  font-weight: bold;
  color: rgb(15, 95, 220);
}

.rightlogo{
      width: 90px;

}

.navbar {
  display: flex;
  gap: 20px;
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

.authButtons {
  display: flex;
  gap: 15px;
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
