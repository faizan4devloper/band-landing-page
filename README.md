
import React, { useState } from "react";
import Modal from "react-modal";
import { useNavigate } from 'react-router-dom';
import styles from "./Header.module.css";
import logoImage from "./HCL Tech.svg";
import RequestDemoForm from "./RequestDemoForm";

const Header = () => {
  const [modalIsOpen, setModalIsOpen] = useState(false);
  const navigate = useNavigate();

  const handleImageClick = () => {
    navigate('/');
  };

  const openModal = () => {
    setModalIsOpen(true);
  };

  const closeModal = () => {
    setModalIsOpen(false);
  };

  return (
    <div className={styles.navbarWrapper}>
      <nav className={styles.header}>
        <div className={styles.logo}>
          <img src={logoImage} alt="" onClick={handleImageClick} style={{cursor:'pointer'}} title="Navigate to Home"/>
        </div>
        <div className={styles.right}>
          <button className={styles.button} onClick={openModal}>Request for live demo</button>
        </div>
      </nav>
      <div className={styles.border}></div>

      <Modal
        isOpen={modalIsOpen}
        onRequestClose={closeModal}
        contentLabel="Request for Live Demo"
        className={styles.modal}
        overlayClassName={styles.overlay}
      >
        <RequestDemoForm closeModal={closeModal} />
      </Modal>
    </div>
  );
};

export default Header;



/* Header.module.css */
.navbarWrapper {
  /* existing styles */
}

.header {
  /* existing styles */
}

.logo {
  /* existing styles */
}

.right {
  /* existing styles */
}

.button {
  /* existing styles */
}

.border {
  /* existing styles */
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
  max-width: 500px;
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
  background-color: rgba(0, 0, 0, 0.75);
  animation: fadeIn 0.3s ease-out;
}





/* RequestDemoForm.module.css */
@keyframes slideIn {
  from {
    transform: translateY(-50px);
    opacity: 0;
  }
  to {
    transform: translateY(0);
    opacity: 1;
  }
}

.formContainer {
  padding: 20px;
  background: #fff;
  border-radius: 8px;
  max-width: 500px;
  margin: 0 auto;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  animation: slideIn 0.5s ease-out;
}

h2 {
  color: #5f1ec1;
  margin-bottom: 20px;
  text-align: center;
  font-size: 24px;
}

.formGroup {
  margin-bottom: 15px;
  position: relative;
}

label {
  display: block;
  margin-bottom: 5px;
  font-weight: 600;
  color: #5f1ec1;
  font-size: 14px;
  transition: color 0.3s;
}

input {
  width: 100%;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
  font-size: 14px;
  transition: border-color 0.3s;
}

input:focus {
  border-color: #5f1ec1;
  outline: none;
}

.submitButton {
  background-color: #5f1ec1;
  color: #fff;
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 16px;
  margin-top: 20px;
  transition: background-color 0.3s;
}

.submitButton:hover {
  background-color: #4a159c;
}

.formGroup input:focus + label {
  color: #4a159c;
}
