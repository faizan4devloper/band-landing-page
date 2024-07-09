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
    <div className={styles.formContainer}>
      <button className={styles.closeButton} onClick={handleCloseModal}>
        <FontAwesomeIcon icon={faTimes} />
      </button>
      <h2 className={styles.demoHead}>Request for a Live Demo</h2>
      {!isSubmitted ? (
        <form onSubmit={handleSubmit}>
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
            <input placeholder="Enter your business details and scope of this demo in your usecase" type="text" required />
          </div>
          <button type="submit" className={styles.submitButton}>Submit</button>
        </form>
      ) : (
        <p className={styles.successMessage}>
          Thank you! Your request for a live demo has been submitted successfully.
        </p>
      )}
    </div>
  );
};

export default RequestDemoForm;




/* RequestDemoForm.module.css */

.formContainer {
  padding: 10px;
  background-color: #f9f9f9;
  border-left: 4px solid rgba(95, 30, 193, 0.8);
  margin-bottom: 20px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  border-radius: 8px;
  max-width: 450px;
  margin: 0 auto;
  animation: slideIn 0.5s ease-out;
}

.demoHead {
  color: #5f1ec1;
  margin-bottom: 10px;
  text-align: center;
  font-size: 18px;
}

.formGroup {
  margin-bottom: 10px;
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

input::placeholder {
  color: #999999; /* Light gray */
  opacity: 1;
}

input {
  width: 90%;
  padding: 4px;
  border: none;
  border-bottom: 1px solid #ccc; /* Only bottom border */
  border-radius: 0;
  font-size: 12px;
  transition: border-color 0.3s;
  font-family: "Poppins", sans-serif; /* Apply Google Font */
}

input:focus {
  border-color: #5f1ec1;
  outline: none;
}

/* Styling for Select */
.select {
  width: 92%;
  font-size: 12px; /* Decrease font size for the Select */
}

.select__control {
  border: 1px solid #5f1ec1;
  border-radius: 4px;
  font-family: "Poppins", sans-serif;
  font-size: 12px; /* Ensure control text is 12px */
  color: #333;
}

.select__control--is-focused {
  border-color: #5f1ec1;
  box-shadow: 0 0 0 1px #5f1ec1;
}

.select__menu {
  font-size: 12px; /* Ensure menu text is 12px */
}

.select__option {
  padding: 8px 8px;
  font-size: 12px; /* Ensure option text is 12px */
}

.submitButton {
  background-color: #5f1ec1;
  color: #fff;
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 14px;
  margin-top: 10px;
  transition: background-color 0.3s;
  margin-left: 30px;
  width: 78%; /* Make the button take the full width */
}

.closeButton {
  position: absolute;
  top: 20px;
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
