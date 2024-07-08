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
          <label>Name</label>
          <input type="text" required />
        </div>
        <div className={styles.formGroup}>
          <label>Email</label>
          <input type="email" required />
        </div>
        <div className={styles.formGroup}>
          <label>Company</label>
          <input type="text" required />
        </div>
        <button type="submit" className={styles.submitButton}>Submit</button>
      </form>
    </div>
  );
};

export default RequestDemoForm;

.formContainer {
  /*padding: 20px;*/
  /*background: #fff;*/
  border-radius: 8px;
  max-width: 500px;
  margin: 0 auto;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
   padding: 10px 15px;
  background-color: #f9f9f9;
  border-left: 4px solid rgba(95, 30, 193, 0.8);
  /*margin-bottom: 20px;*/
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  /*margin-bottom: 50px;*/
}

.demoHead{
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
  width: 70%;
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
