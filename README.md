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

.navbarWrapper {
  position: fixed;
  top: 0;
  left: 0;
  min-width: 100%;
  background-color: #fff;
  border-bottom: 0.1px solid rgba(219, 197, 255, 1);
  padding: 8px 0px;
  z-index: 0;
  transition: transform 0.5s ease-in-out;

  /*margin-bottom: 30px;*/
}
.navbarWrapper.hide {
  transform: translateY(-100%);
}

.navbarWrapper.show {
  transform: translateY(0);
}
.navbarWrapper .header {
  display: flex;
  align-items: center;
  justify-content: space-around;
  margin: 5px -200px;
}
.logo img {
  max-height: 20px;
}

.right {
  display: flex;
  align-items: center;
}

.button {
  display: flex;
  align-items: center;
  background: linear-gradient(90deg, #6f36cd 0%, #1f77f6 100%);
  color: white;
  border: none;
  padding: 8px 8px;
  font-size: 12px;
  border-radius: 5px;
  cursor: pointer;
  transition: .6s ease;
}

.button:hover{
    transform: translate(0, -5px);
    
    

}

.content {
  padding-top: 70px; /* Adjust the value to match the height of your header */
}

.logo img::after {
  content: attr(title);
  position: absolute;
  top: 100%;
  left: 50%;
  transform: translateX(-50%);
  background-color: #333;
  color: #fff;
  padding: 5px;
  border-radius: 4px;
  font-size: 12px;
  white-space: nowrap;
  opacity: 0;
  visibility: hidden;
  transition: opacity 0.3s, visibility 0.1s;
}

.logo img:hover::after {
  opacity: 1;
  visibility: visible;
}

@keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}

.modal {
  position: absolute;
  top: 50%;
  left: 50%;
  right: auto;
  bottom: auto;
  transform: translate(-50%, -50%);
  background: #fff;
  border-radius: 8px;
  padding: 20px;
  margin-top: 30px;
  max-width: 900px;
  width: 90%;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  animation: fadeIn 0.3s ease-out;
}

.overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.9);
  animation: fadeIn 0.3s ease-out;
}
