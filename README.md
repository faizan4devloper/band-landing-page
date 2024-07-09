import React, { useState } from "react";
import Select from "react-select";
import styles from "./RequestDemoForm.module.css";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faTimes } from "@fortawesome/free-solid-svg-icons";

const genAISolutions = [
  { value: "solution1", label: "Email EAR" },
  { value: "solution2", label: "Code GReat" },
  { value: "solution3", label: "Case Intelligence" },
  // Add more solutions here
];

const RequestDemoForm = ({ closeModal }) => {
  const [isSubmitted, setIsSubmitted] = useState(false);
  const [selectedSolution, setSelectedSolution] = useState(null);

  const handleSubmit = (e) => {
    e.preventDefault();
    // Handle form submission logic here
    setIsSubmitted(true);
  };

  const handleCloseModal = () => {
    setIsSubmitted(false); // Reset submission state
    closeModal();
  };

  return (
    <div className={styles.modal}>
      <div className={styles.formContainer}>
        <button className={styles.closeButton} onClick={handleCloseModal}>
          <FontAwesomeIcon icon={faTimes} />
        </button>
        <h2 className={styles.demoHead}>Request for a Live Demo</h2>
        {!isSubmitted ? (
          <form onSubmit={handleSubmit} className={styles.form}>
            <div className={styles.formGroup}>
              <label>*User Name</label>
              <input type="text" required />
            </div>
            <div className={styles.formGroup}>
              <label>*Email Address</label>
              <input type="email" required />
            </div>
            <div className={styles.formGroup}>
              <label>GenAI Solution Name</label>
              <Select
                options={genAISolutions}
                value={selectedSolution}
                onChange={setSelectedSolution}
                className={styles.select}
                classNamePrefix="select"
                placeholder="Select a solution"
                isClearable
              />
            </div>
            <div className={styles.formGroup}>
              <label>Domain</label>
              <input type="text" required />
            </div>
            <div className={styles.formGroup}>
              <label>Customer Name</label>
              <input type="text" required />
            </div>
            <div className={styles.formGroup}>
              <label>More Details</label>
              <textarea
                placeholder="Enter your business details and scope of this demo in your use case. More details here."
                rows="4"
                required
              ></textarea>
            </div>
            <button type="submit" className={styles.submitButton}>
              Submit
            </button>
          </form>
        ) : (
          <p className={styles.successMessage}>
            Thank you! Your request for a live demo has been submitted
            successfully.
          </p>
        )}
      </div>
    </div>
  );
};

export default RequestDemoForm;


.modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  background: rgba(0, 0, 0, 0.5);
  z-index: 1000;
  overflow-y: auto; /* Allow vertical scrolling */
}

.formContainer {
  padding: 20px;
  background-color: #f9f9f9;
  border-left: 4px solid rgba(95, 30, 193, 0.8);
  margin: 20px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  border-radius: 8px;
  max-width: 500px;
  width: 100%;
  max-height: 90vh; /* Set max height to 90% of viewport height */
  overflow-y: auto; /* Allow scrolling if content exceeds container height */
  position: relative;
  animation: slideIn 0.5s ease-out;
}

.demoHead {
  color: #5f1ec1;
  margin-bottom: 20px;
  text-align: center;
  font-size: 18px;
}

.form {
  display: flex;
  flex-direction: column;
}

.formGroup {
  margin-bottom: 15px;
  position: relative;
}

label {
  display: block;
  margin-bottom: 5px;
  font-weight: 600;
  color: #000;
  font-size: 12px;
  transition: color 0.3s;
}

input::placeholder,
textarea::placeholder {
  color: #999999;
  opacity: 1;
}

input,
textarea {
  width: 100%;
  padding: 8px;
  border: none;
  border-bottom: 1px solid #ccc;
  border-radius: 0;
  font-size: 14px;
  transition: border-color 0.3s;
  font-family: "Poppins", sans-serif;
}

input:focus,
textarea:focus {
  border-color: #5f1ec1;
  outline: none;
}

/* Styling for Select */
.select {
  width: 100%;
}

.select__control {
  border: 1px solid #5f1ec1 !important;
  border-radius: 4px !important;
  font-family: "Poppins", sans-serif !important;
  font-size: 14px !important;
  color: #333 !important;
}

.select__control--is-focused {
  border-color: #5f1ec1 !important;
  box-shadow: 0 0 0 1px #5f1ec1 !important;
}

.select__menu {
  font-size: 14px !important;
}

.select__option {
  padding: 8px 8px !important;
  font-size: 14px !important;
}

.select__option--is-focused {
  background-color: #5f1ec1 !important;
  color: white !important;
}

.select__option--is-selected {
  background-color: #5f1ec1 !important;
  color: white !important;
}

.submitButton {
  background-color: #5f1ec1;
  color: #fff;
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 14px;
  transition: background-color 0.3s;
  width: 100%;
}

.closeButton {
  position: absolute;
  top: 10px;
  right: 10px;
  background-color: transparent;
  border: none;
  cursor: pointer;
  font-size: 24px;
  color: #aaa;
}

.closeButton:hover {
  color: #555;
}

.successMessage {
  text-align: center;
  color: #5f1ec1;
  font-size: 16px;
}
