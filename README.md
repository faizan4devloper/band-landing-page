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
      <h2 className={styles.demoHead}>Request for Live Demo</h2>
      <form onSubmit={handleSubmit}>
        <div className={styles.formGroup}>
          <label>User Name</label>
          <input type="text" required />
        </div>
        <div className={styles.formGroup}>
          <label>Email Address</label>
          <input type="email" required />
        </div>
        <div className={styles.formGroup}>
          <label>Solution Name</label>
          <input type="text" required />
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
          <input placeholder="Moew Details" type="text" required />
        </div>
        <button type="submit" className={styles.submitButton}>Submit</button>
      </form>
    </div>
  );
};

export default RequestDemoForm;



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
   padding: 10px 15px;
  background-color: #f9f9f9;
  border-left: 4px solid rgba(95, 30, 193, 0.8);
  margin-bottom: 20px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  border-radius: 8px;
  max-width: 500px;
  margin: 0 auto;
  /*box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);*/
  animation: slideIn 0.5s ease-out;
}

.demoHead {
  color: #5f1ec1;
  margin-bottom: 20px;
  text-align: center;
  font-size: 20px;
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

input {
  width: 80%;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
  font-size: 12px;
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
  background-color: #f9f9f9;
  border-left: 4px solid rgba(95, 30, 193, 0.8);
  margin-bottom: 20px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  border-radius: 8px;
  max-width: 500px;
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

input {
  width: 100%;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
  font-size: 12px;
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
  font-size: 14px;
  margin-top: 10px;
  transition: background-color 0.3s;
  width: 100%; /* Make the button take the full width */
}

.submitButton:hover {
  background-color: #4a159c;
}

.formGroup input:focus + label {
  color: #4a159c;
}
