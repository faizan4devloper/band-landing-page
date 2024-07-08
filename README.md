
// RequestDemoForm.js
import React from "react";
import styles from "./RequestDemoForm.module.css";

const RequestDemoForm = ({ closeModal }) => {
  const handleSubmit = (e) => {
    e.preventDefault();
    // Handle form submission logic here
    closeModal();
  };

  return (
    <div className={styles.formContainer}>
      <h2>Request for Live Demo</h2>
      <form onSubmit={handleSubmit}>
        <div className={styles.formGroup}>
          <label>Name:</label>
          <input type="text" required />
        </div>
        <div className={styles.formGroup}>
          <label>Email:</label>
          <input type="email" required />
        </div>
        <div className={styles.formGroup}>
          <label>Company:</label>
          <input type="text" required />
        </div>
        <button type="submit" className={styles.submitButton}>Submit</button>
      </form>
    </div>
  );
};

export default RequestDemoForm;


/* RequestDemoForm.module.css */
.formContainer {
  padding: 20px;
  background: #fff;
  border-radius: 8px;
  max-width: 500px;
  margin: 0 auto;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

h2 {
  color: #5f1ec1;
}

.formGroup {
  margin-bottom: 15px;
}

label {
  display: block;
  margin-bottom: 5px;
  font-weight: 600;
}

input {
  width: 100%;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.submitButton {
  background-color: #5f1ec1;
  color: #fff;
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 16px;
}


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
}

.overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.75);
}
