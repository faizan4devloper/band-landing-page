import React, { useState } from "react";
import Select from "react-select";
import styles from "./RequestDemoForm.module.css";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faTimes } from "@fortawesome/free-solid-svg-icons";

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

  const customStyles = {
    control: (provided, state) => ({
      ...provided,
      borderRadius: "4px",
      width: "91%", // Control width
      border: state.isFocused ? "1px solid #5f1ec1" : "1px solid #ccc",
      cursor: "pointer",
      backgroundColor: "#fff",
      color: "#555",
      boxShadow: "none", // Remove the default box-shadow
      "&:hover": {
        border: state.isFocused ? "1px solid #5f1ec1" : "1px solid #ccc"
      }
    }),
    option: (provided, state) => ({
      ...provided,
      backgroundColor: state.isSelected ? "#5f1ec1" : state.isFocused ? "#eee" : "#fff",
      color: state.isSelected ? "#fff" : "#555",
      fontSize: "12px", // Option font size
      width: "100%", // Option width
      cursor: "pointer",
      "&:hover": {
        backgroundColor: state.isSelected ? "#5f1ec1" : "#f0f0f0" // Change hover background color
      }
    }),
    placeholder: (provided) => ({
      ...provided,
      fontSize: "12px", // Placeholder font size
      color: "#999" // Placeholder color
    }),
    menu: (provided) => ({
      ...provided,
      width: "91%",
      maxHeight: "200px", // Limit the height of the menu to 200px
      overflowY: "auto", 
      zIndex: 2 // Ensure it is above other elements
    }),
    singleValue: (provided) => ({
      ...provided,
      fontSize: "12px",
      color: "#555" // Single value color
    })
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
            <div className={styles.customSelect}>
              <Select
                styles={customStyles}
                value={selectedSolution}
                onChange={setSelectedSolution}
                options={[
                  { value: "option1", label: "Intelligent Assist" },
                  { value: "option2", label: "Email EAR" },
                  { value: "option3", label: "Case Intelligence" },
                  { value: "option4", label: "Smart Recruit" },
                  { value: "option5", label: "iAssure Claim" },
                  { value: "option6", label: "Assistant For EVs" },
                  { value: "option7", label: "AutoWise Companion" },
                  { value: "option8", label: "Citizen Advisor" },
                  { value: "option9", label: "Fin Competitor Summary Gen" },
                  { value: "option10", label: "Signature Extraction & Verification" },
                  { value: "option11", label: "AI Force" },
                  { value: "option12", label: "API based Test Case Generation" },
                  { value: "option13", label: "AMS Support Automation" },
                  { value: "option14", label: "SOP Assistance" },
                  { value: "option15", label: "Code GReat" },
                  { value: "option16", label: "AAIG-API Analyzer & Insight Generator" },
                  { value: "option17", label: "Responsible Gen AI with Llama-13 B" },
                  { value: "option18", label: "Graph data Interpretation using Gen AI" },
                  { value: "option19", label: "Predictive Asset Maintenance​(PAM)​" }
                ]}
              />
            </div>
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





import React, { useState } from "react";
import Select from "react-select";
import styles from "./RequestDemoForm.module.css";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faTimes } from "@fortawesome/free-solid-svg-icons";

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

  const customStyles = {
    control: (provided, state) => ({
      ...provided,
      borderRadius: "4px",
      width: "91%", // Control width
      border: state.isFocused ? "1px solid #5f1ec1" : "1px solid #ccc",
      cursor: "pointer",
      backgroundColor: "#fff",
      color: "#555",
      boxShadow: "none", // Remove the default box-shadow
      "&:hover": {
        border: state.isFocused ? "1px solid #5f1ec1" : "1px solid #ccc"
      }
    }),
    option: (provided, state) => ({
      ...provided,
      backgroundColor: state.isSelected ? "#5f1ec1" : state.isFocused ? "#eee" : "#fff",
      color: state.isSelected ? "#fff" : "#555",
      fontSize: "12px", // Option font size
      width: "100%", // Option width
      cursor: "pointer",
      "&:hover": {
        backgroundColor: state.isSelected ? "#5f1ec1" : "#f0f0f0" // Change hover background color
      }
    }),
    placeholder: (provided) => ({
      ...provided,
      fontSize: "12px", // Placeholder font size
      color: "#999" // Placeholder color
    }),
    menu: (provided) => ({
      ...provided,
      width: "91%",
      maxHeight: "300px", // Limit the height of the menu to 150px (approx. 5 options)
      overflowY: "auto", 
      zIndex: 2 // Ensure it is above other elements
    }),
    singleValue: (provided) => ({
      ...provided,
      fontSize: "12px",
      color: "#555" // Single value color
    })
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
            <div className={styles.customSelect}>
              <Select
                styles={customStyles}
                value={selectedSolution}
                onChange={setSelectedSolution}
                options={[
                  { value: "option1", label: "Intelligent Assist" },
                  { value: "option2", label: "Email EAR" },
                  { value: "option3", label: "Case Intelligence" },
                  { value: "option4", label: "Smart Recruit" },
                  { value: "option5", label: "iAssure Claim" },
                  { value: "option6", label: "Assistant For EVs" },
                  { value: "option7", label: "AutoWise Companion" },
                  { value: "option8", label: "Citizen Advisor" },
                  { value: "option9", label: "Fin Competitor Summary Gen" },
                  { value: "option10", label: "Signature Extraction & Verification" },
                  { value: "option11", label: "AI Force" },
                  { value: "option12", label: "API based Test Case Generation" },
                  { value: "option13", label: "AMS Support Automation" },
                  { value: "option14", label: "SOP Assistance" },
                  { value: "option15", label: "Code GReat" },
                  { value: "option16", label: "AAIG-API Analyzer & Insight Generator" },
                  { value: "option17", label: "Responsible Gen AI with Llama-13 B" },
                  { value: "option18", label: "Graph data Interpretation using Gen AI" },
                  { value: "option19", label: "Predictive Asset Maintenance​(PAM)​" }
                ]}
              />
            </div>
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
