import React, { useState, useEffect, useRef } from "react";
import Modal from "react-modal";
import { useNavigate } from "react-router-dom";
import styles from "./Header.module.css";
import { useHeaderContext } from '../Context/HeaderContext'; // Import context
import logoImage from "./HCLTechLogo.svg";
import RequestDemoForm from "./RequestDemoForm";
import laserStyles from "./LaserCursor.module.css";

const Header = () => {
  const { headerZIndex, setHeaderZIndex } = useHeaderContext(); // Use context
  const [modalIsOpen, setModalIsOpen] = useState(false);
  const [isVisible, setIsVisible] = useState(true);
  const [isLaserEnabled, setIsLaserEnabled] = useState(false);
  const [cursorPosition, setCursorPosition] = useState({ x: 0, y: 0 });

  const [lastScrollY, setLastScrollY] = useState(0);
  const navigate = useNavigate();
  const laserCursorRef = useRef(null);

  const handleImageClick = () => {
    navigate("/");
  };
  
  useEffect(() => {
    setHeaderZIndex(1000); // Set zIndex when component mounts
  }, [setHeaderZIndex]);


  const openModal = () => {
    setModalIsOpen(true);
  };

  const closeModal = () => {
    setModalIsOpen(false);
  };

  const controlHeaderVisibility = () => {
    if (window.scrollY > lastScrollY) {
      setIsVisible(false);
    } else {
      setIsVisible(true);
    }
    setLastScrollY(window.scrollY);
  };

  const toggleLaserCursor = () => {
    setIsLaserEnabled((prev) => !prev);
  };

  useEffect(() => {
    window.addEventListener("scroll", controlHeaderVisibility);
    return () => {
      window.removeEventListener("scroll", controlHeaderVisibility);
    };
  }, [lastScrollY]);

  useEffect(() => {
    const handleMouseMove = (e) => {
      setCursorPosition({ x: e.clientX, y: e.clientY });
    };

    if (isLaserEnabled) {
      document.body.classList.add("laser-enabled");
      document.body.style.cursor = 'none'; // Force hide default cursor
      window.addEventListener("mousemove", handleMouseMove);
    } else {
      document.body.classList.remove("laser-enabled");
      document.body.style.cursor = ''; // Reset cursor style
      window.removeEventListener("mousemove", handleMouseMove);
    }

    return () => {
      window.removeEventListener("mousemove", handleMouseMove);
    };
  }, [isLaserEnabled]);

  return (
    <div className={`${styles.navbarWrapper} ${isVisible ? styles.show : styles.hide}`} style={{ zIndex: headerZIndex }}>
      <nav className={styles.header}>
        <div className={styles.logo}>
          <img
            src={logoImage}
            alt=""
            onClick={handleImageClick}
            style={{ cursor: "pointer" }}
            title="Navigate to Home"
          />
        </div>
        <div className={styles.right}>
          <button className={styles.button} onClick={openModal}>
            Request For Live Demo
          </button>
          <button className={styles.button} onClick={toggleLaserCursor}>
            {isLaserEnabled ? "Disable Laser Cursor" : "Enable Laser Cursor"}
          </button>
        </div>
      </nav>
      <div className={styles.border}></div>

      <Modal
        isOpen={modalIsOpen}
        onRequestClose={closeModal}
        contentLabel="Request for Live Demo!"
        className={styles.modal}
        overlayClassName={styles.overlay}
      >
        <RequestDemoForm closeModal={closeModal} />
      </Modal>

      {isLaserEnabled && (
        <div
          ref={laserCursorRef}
          className={laserStyles.laserCursor}
          style={{
            left: `${cursorPosition.x}px`,
            top: `${cursorPosition.y}px`,
            transform: `translate(-50%, -50%)`, // Center the cursor on the pointer
          }}
        />
      )}
    </div>
  );
};

export default Header;

