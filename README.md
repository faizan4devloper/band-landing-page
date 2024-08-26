import React, { useState, useEffect } from "react";
import Modal from "react-modal";
import { useNavigate } from "react-router-dom";
import styles from "./Header.module.css";
import logoImage from "./HCLTechLogo.svg";
import RequestDemoForm from "./RequestDemoForm";

const Header = () => {
  const [modalIsOpen, setModalIsOpen] = useState(false);
  const [isVisible, setIsVisible] = useState(true);
  const [lastScrollY, setLastScrollY] = useState(0);
  const navigate = useNavigate();

  const handleImageClick = () => {
    navigate("/");
  };

  const openModal = () => {
    setModalIsOpen(true);
  };

  const closeModal = () => {
    setModalIsOpen(false);
  };

  const controlHeaderVisibility = () => {
    if (window.scrollY > lastScrollY) {
      // Scrolling down
      setIsVisible(false);
    } else {
      // Scrolling up
      setIsVisible(true);
    }
    setLastScrollY(window.scrollY);
  };

  useEffect(() => {
    window.addEventListener("scroll", controlHeaderVisibility);
    return () => {
      window.removeEventListener("scroll", controlHeaderVisibility);
    };
  }, [lastScrollY]);

  return (
    <div className={`${styles.navbarWrapper} ${isVisible ? styles.show : styles.hide}`}>
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
    </div>
  );
};

export default Header;
