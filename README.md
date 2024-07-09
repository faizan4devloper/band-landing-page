import React, { useState } from "react";
import Select from "react-select";
import styles from "./CustomSelect.module.css"; // Import your CSS module

const customStyles = {
  control: (provided) => ({
    ...provided,
    border: "1px solid #5f1ec1",
    borderRadius: "4px",
    fontSize: "12px",
    minHeight: "32px",
  }),
  option: (provided, state) => ({
    ...provided,
    backgroundColor: state.isSelected ? "#5f1ec1" : "white",
    color: state.isSelected ? "white" : "#333",
    fontSize: "12px",
    cursor: "pointer",
  }),
  menu: (provided) => ({
    ...provided,
    fontSize: "12px",
  }),
};

const genAISolutions = [
  { value: "solution1", label: "Email EAR" },
  { value: "solution2", label: "Code GReat" },
  { value: "solution3", label: "Case Intelligence" },
  // Add more solutions here
];

const CustomSelect = ({ selectedOption, onChange }) => {
  const [isOpen, setIsOpen] = useState(false);

  const toggleMenu = () => {
    setIsOpen(!isOpen);
  };

  return (
    <Select
      styles={customStyles}
      className={styles.customSelect}
      classNamePrefix="custom-select"
      value={selectedOption}
      onChange={onChange}
      options={genAISolutions}
      isClearable
      isSearchable
      menuIsOpen={isOpen}
      onMenuOpen={toggleMenu}
      onMenuClose={toggleMenu}
    />
  );
};

export default CustomSelect;


/* CustomSelect.module.css */

.custom-select__control {
  border: 1px solid #5f1ec1 !important;
  border-radius: 4px !important;
  font-family: "Poppins", sans-serif !important;
  font-size: 12px !important;
  min-height: 32px !important;
}

.custom-select__option {
  font-size: 12px !important;
}

.custom-select__menu {
  font-size: 12px !important;
}

import React, { useState } from "react";
import Select from "react-select";
import styles from "./RequestDemoForm.module.css";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faTimes } from "@fortawesome/free-solid-svg-icons";
import CustomSelect from "./CustomSelect"; // Import your custom select component

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
            <CustomSelect
              selectedOption={selectedSolution}
              onChange={setSelectedSolution}
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
              placeholder="Enter your business details and scope of this demo in your usecase."
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
  );
};

export default RequestDemoForm;
