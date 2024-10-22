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
      <div className={styles.logoContainer} onclick={handleImageClick}>
        <img src={logo} alt="Logo" className={styles.logo}/>
      </div>
      
    </header>
  );
};

export default Header;
