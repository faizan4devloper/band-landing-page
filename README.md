import React, { useState, useEffect } from 'react';
import styles from './Header.module.css';
import logo from '../assets/HCLTechLogoWhite.png'; // Ensure this path points to your logo
import { useNavigate } from "react-router-dom";

const Header = () => {
  const [prevScrollPos, setPrevScrollPos] = useState(window.pageYOffset);
  const [visible, setVisible] = useState(true);
  const [headerClass, setHeaderClass] = useState(styles.headerVisible); // Default class for visible state
  const navigate = useNavigate();

  const handleImageClick = () => {
    navigate("/");
  };

  useEffect(() => {
    const handleScroll = () => {
      const currentScrollPos = window.pageYOffset;
      const isScrollingUp = prevScrollPos > currentScrollPos;

      setVisible(isScrollingUp || currentScrollPos < 100); // Header is visible if scrolling up or near the top

      setPrevScrollPos(currentScrollPos);
    };

    window.addEventListener('scroll', handleScroll);
    
    // Cleanup the event listener on component unmount
    return () => window.removeEventListener('scroll', handleScroll);
  }, [prevScrollPos]);

  // Update header class based on visibility state
  useEffect(() => {
    setHeaderClass(visible ? styles.headerVisible : styles.headerHidden);
  }, [visible]);

  return (
    <header className={`${styles.header} ${headerClass}`}>
      <div className={styles.logoContainer} onClick={handleImageClick}>
        <img src={logo} alt="Logo" className={styles.logo} />
      </div>
      <div className={styles.citizenAdvisor} onClick={handleImageClick}>
        CitizenAdvisor
      </div>
    </header>
  );
};

export default Header;





.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem 6rem;
  background: linear-gradient(90deg, rgb(95, 30, 193) 0%, rgb(15, 95, 220) 100%);
  border-bottom: 0.1px solid rgb(62, 62, 62);
  transition: transform 0.5s ease-in-out, opacity 0.5s ease-in-out, box-shadow 0.5s ease-in-out;
  color: white;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  z-index: 10;
  position: fixed;
  width: 100%;
  top: 0;
  left: 0;
}

.headerVisible {
  transform: translateY(0); /* Visible state */
  opacity: 1;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1); /* Regular shadow */
  z-index: 10;
}

.headerHidden {
  transform: translateY(-100%); /* Hide header by moving it up */
  opacity: 0;
  box-shadow: none; /* Remove shadow when hidden */
  z-index: -1; /* Lower z-index when hidden */
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

.citizenAdvisor {
  font-size: 1.1rem;
  font-weight: 700;
  color: #fff;
  letter-spacing: 0.05rem;
  cursor: pointer;
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
