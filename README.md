import React from 'react';
import styles from './Header.module.css';
import logo from '../assets/HCLTechLogoWhite.png'; // Ensure this path points to your logo
import { useNavigate } from "react-router-dom";



const Header = () => {
  
  const navigate = useNavigate();

  const handleImageClick = () => {
    navigate("/");
  };
  
  return (
    <header className={styles.header}>
      <div className={styles.logoContainer} onClick={handleImageClick}>
        <img src={logo} alt="Logo" className={styles.logo}/>
      </div>
      
      <div>
        Citizen Advisor
      </div>
      
    </header>
  );
};

export default Header;




/* Header.module.css */

.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem 6rem;
/*background: linear-gradient(rgb(26, 26, 46), rgb(22, 33, 62)) 0% 0% repeat rgba(0, 0, 0, 0) ;*/
/*background:#411482;*/
/*background:#4c189e;*/
background:linear-gradient(90deg, rgb(95, 30, 193) 0%, rgb(15, 95, 220) 100%) 0% 0% repeat rgba(0, 0, 0, 0);
    border-bottom: 0.1px solid rgb(62, 62, 62);
transition-duration: 0.5s;
    transition-timing-function: ease-in-out;
    transition-property: transform;
color: white;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}


    

.logoContainer {
  display: flex;
  align-items: center;
  cursor: pointer;
}

.logo {
  width: 100px;
  margin-right: 0.75rem;
}

.logoText {
  font-size: 1.5rem;
  font-weight: bold;
  letter-spacing: 1px;
}

.navbar {
  display: flex;
}

.navLinks {
  display: flex;
  list-style: none;
  gap: 1.5rem;
}

.navItem a {
  text-decoration: none;
  color: white;
  font-weight: 500;
  transition: color 0.3s ease;
}

.navItem a:hover {
  color: #e0e7ff; /* Light shade on hover */
}
