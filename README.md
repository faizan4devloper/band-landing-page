import React, { useEffect, useRef } from "react";
import styles from "./Header.module.css";
import { gsap } from "gsap";

import Logo from '../assets/HCLTechLogoBlue.svg';

const Header = () => {
  const logoRef = useRef(null);
  const rightLogoRef = useRef(null);

  useEffect(() => {
    // GSAP Animations
    gsap.fromTo(
      logoRef.current,
      { opacity: 0, scale: 0.5 },
      {
        opacity: 1,
        scale: 1,
        duration: 1.5,
        ease: "elastic.out(1, 0.5)",
      }
    );

    gsap.fromTo(
      rightLogoRef.current,
      { x: 50, opacity: 0 },
      { x: 0, opacity: 1, duration: 1, ease: "power2.out", delay: 0.5 }
    );
  }, []);

  return (
    <header className={styles.header}>
      <div ref={logoRef} className={styles.logoContainer}>
        <span className={styles.logo}>Claim Assist</span>
      </div>
      <div className={styles.authButtons}>
        <img
          ref={rightLogoRef}
          className={styles.rightLogo}
          src={Logo}
          alt="HCL Logo"
        />
      </div>
    </header>
  );
};

export default Header;



/* Header Layout */
.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 20px 80px;
  background-color: #ffffff;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  position: relative;
  overflow: hidden;
}

/* Logo Styling */
.logoContainer {
  position: relative;
}

.logo {
  font-size: 2rem;
  font-weight: bold;
  color: #0f5fdc;
  background: linear-gradient(90deg, #0f5fdc, #7ca2e1, #0f5fdc);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}

/* Right Logo Hover Animation */
.rightLogo {
  width: 90px;
  transition: transform 0.4s ease, box-shadow 0.4s ease;
}

.rightLogo:hover {
  transform: scale(1.1);
  box-shadow: 0 8px 15px rgba(15, 95, 220, 0.3);
}

/* Auth Buttons Section */
.authButtons {
  display: flex;
  align-items: center;
}
