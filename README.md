import React, { useState } from "react";
import axios from "axios";
import Select from "react-select";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faStar } from "@fortawesome/free-solid-svg-icons";
import styles from "./FeedbackForm.module.css";

const FeedbackForm = ({ closeModal }) => {
  const [isSubmitted, setIsSubmitted] = useState(false);
  const [selectedSolution, setSelectedSolution] = useState(null);
  const [rating, setRating] = useState(0);
  const [formData, setFormData] = useState({
    userName: '',
    email: '',
    feedback: ''
  });

  const handleInputChange = (e) => {
    setFormData({
      ...formData,
      [e.target.name]: e.target.value
    });
  };

  const handleStarClick = (newRating) => {
    setRating(newRating);
  };

  const handleSubmit = async (e) => {
    e.preventDefault();

    try {
      // Send the feedback data to the server or API
      await axios.post('YOUR_API_ENDPOINT', {
        ...formData,
        rating,
        solution: selectedSolution ? selectedSolution.label : ''
      });

      setIsSubmitted(true);
    } catch (error) {
      console.error('Error submitting feedback:', error);
      // Handle error case
    }
  };

  const customStyles = {
    control: (provided, state) => ({
      ...provided,
      borderRadius: "4px",
      width: "91%",
      border: state.isFocused ? "1px solid #5f1ec1" : "1px solid #ccc",
      cursor: "pointer",
      backgroundColor: "#fff",
      boxShadow: "none"
    }),
    option: (provided, state) => ({
      ...provided,
      backgroundColor: state.isSelected ? "#5f1ec1" : state.isFocused ? "#eee" : "#fff",
      color: state.isSelected ? "#fff" : "#555"
    })
  };

  return (
    <div className={styles.formContainer}>
      <button className={styles.closeButton} onClick={closeModal}>
        &times;
      </button>
      <h2 className={styles.feedbackHead}>Provide Your Feedback</h2>
      {!isSubmitted ? (
        <form onSubmit={handleSubmit}>
          <div className={styles.formGroup}>
            <label>*User Name</label>
            <input
              type="text"
              name="userName"
              value={formData.userName}
              onChange={handleInputChange}
              required
            />
          </div>
          <div className={styles.formGroup}>
            <label>*Email Address</label>
            <input
              type="email"
              name="email"
              value={formData.email}
              onChange={handleInputChange}
              required
            />
          </div>
          <div className={styles.formGroup}>
            <label>Select Solution/Portal</label>
            <div className={styles.customSelect}>
              <Select
                styles={customStyles}
                value={selectedSolution}
                onChange={setSelectedSolution}
                options={[
                  { value: "solution1", label: "Solution 1" },
                  { value: "solution2", label: "Solution 2" }
                ]}
              />
            </div>
          </div>
          <div className={styles.formGroup}>
            <label>Feedback</label>
            <textarea
              name="feedback"
              value={formData.feedback}
              onChange={handleInputChange}
              rows="4"
              required
            />
          </div>
          <div className={styles.ratingGroup}>
            <label>Rate Us</label>
            <div className={styles.starRating}>
              {[1, 2, 3, 4, 5].map((star) => (
                <FontAwesomeIcon
                  key={star}
                  icon={faStar}
                  className={star <= rating ? styles.activeStar : styles.inactiveStar}
                  onClick={() => handleStarClick(star)}
                />
              ))}
            </div>
          </div>
          <button type="submit" className={styles.submitButton}>Submit</button>
        </form>
      ) : (
        <p className={styles.successMessage}>
          Thank you for your feedback!
        </p>
      )}
    </div>
  );
};

export default FeedbackForm;



.formContainer {
  padding: 30px;
  background-color: #f9f9f9;
  border-left: 4px solid rgba(95, 30, 193, 0.8);
  margin-bottom: 20px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  border-radius: 8px;
  max-width: 450px;
  margin: 0 auto;
}

.feedbackHead {
  color: #5f1ec1;
  margin-bottom: 10px;
  text-align: center;
  font-size: 18px;
}

.formGroup {
  margin-bottom: 10px;
}

label {
  display: block;
  margin-bottom: 5px;
  font-weight: 600;
  font-size: 12px;
}

input, textarea {
  width: 90%;
  padding: 5px;
  border: none;
  border-bottom: 1px solid #ccc;
  font-size: 12px;
}

.starRating {
  display: flex;
  justify-content: flex-start;
  gap: 5px;
}

.activeStar {
  color: #f8c22e;
  cursor: pointer;
}

.inactiveStar {
  color: #ccc;
  cursor: pointer;
}

.submitButton {
  background-color: #5f1ec1;
  color: #fff;
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 14px;
  width: 78%;
}

.successMessage {
  text-align: center;
  color: #5f1ec1;
  font-size: 16px;
}










import React, { useState } from "react";
import axios from "axios";
import Select from "react-select";
import styles from "./RequestDemoForm.module.css";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faTimes } from "@fortawesome/free-solid-svg-icons";

const RequestDemoForm = ({ closeModal }) => {
  const [isSubmitted, setIsSubmitted] = useState(false);
  const [selectedSolution, setSelectedSolution] = useState(null);
  const [formData, setFormData] = useState({
    userName: '',
    email: '',
    domain: '',
    customerName: '',
    details: ''
  });

  const handleInputChange = (e) => {
    setFormData({
      ...formData,
      [e.target.name]: e.target.value
    });
  };

  const handleSubmit = async (e) => {
    e.preventDefault();

    try {
      // Send the form data to the Lambda function through the API Gateway
      await axios.post('XYZ', {
        ...formData,
        solution: selectedSolution ? selectedSolution.label : ''
      });

      setIsSubmitted(true);
    } catch (error) {
      console.error('Error submitting form:', error);
      // Handle error case
    }
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
       // Limit the height of the menu to 150px
      overflowY: "auto", // Enable vertical scrolling when necessary
      zIndex: 2 // Ensure it is above other elements
    }),
    menuList: (provided) => ({
      ...provided,
      maxHeight: "150px",
      padding: 0 // Remove padding to avoid additional scrolling issues
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
            <input
              type="text"
              name="userName"
              value={formData.userName}
              onChange={handleInputChange}
              required
            />
          </div>
          <div className={styles.formGroup}>
            <label>*Email Address</label>
            <input
              type="email"
              name="email"
              value={formData.email}
              onChange={handleInputChange}
              required
            />
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
                  { value: "option19", label: "Predictive Asset Maintenance​(PAM)​" },
                ]}
              />
            </div>
          </div>
          <div className={styles.formGroup}>
            <label>Domain</label>
            <input
              type="text"
              name="domain"
              value={formData.domain}
              onChange={handleInputChange}
              required
            />
          </div>
          <div className={styles.formGroup}>
            <label>Customer Name</label>
            <input
              type="text"
              name="customerName"
              value={formData.customerName}
              onChange={handleInputChange}
              required
            />
          </div>
          <div className={styles.formGroup}>
            <label>More Details</label>
            <textarea
              name="details"
              placeholder="Enter your business details and scope of this demo in your use case."
              rows="4"
              value={formData.details}
              onChange={handleInputChange}
              required
            ></textarea>
          </div>
          <button type="submit" className={styles.submitButton}>
            Submit
          </button>
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

.formContainer {
  padding: 30px;
  background-color: #f9f9f9;
  border-left: 4px solid rgba(95, 30, 193, 0.8);
  margin-bottom: 20px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  border-radius: 8px;
  max-width: 450px;
  margin: 0 auto;
  animation: slideIn 0.5s ease-out;
  max-height: 80vh; /* Set a max-height for the form container */
  z-index: 1000;
}

.demoHead {
  color: #5f1ec1;
  margin-bottom: 10px;
  text-align: center;
  margin-top: -10px;
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

input::placeholder, textarea::placeholder {
  color: #999999; /* Light gray */
  opacity: 1;
}

input, textarea {
  width: 90%;
  padding: 5px;
  border: none;
  background-color: transparent;
  border-bottom: 1px solid #ccc; /* Only bottom border */
  border-radius: 0;
  font-size: 12px;
  transition: border-color 0.3s;
  font-family: "Poppins", sans-serif; /* Apply Google Font */
}

input:focus, textarea:focus {
  border-color: #5f1ec1;
  outline: none;
}

.submitButton {
  background-color: #5f1ec1;
  margin-top: 0px;
  color: #fff;
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 14px;
  margin-top: 10px;
  width: 78%; /* Make the button take the full width */
}

.closeButton {
  position: absolute;
  top: 25px;
  right: 25px;
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

/* Custom scrollbar styling for the form container */
/*.formContainer::-webkit-scrollbar {*/
/*  width: 6px;*/
/*}*/

/*.formContainer::-webkit-scrollbar-track {*/
/*  background: #f1f1f1;*/
/*  border-radius: 8px;*/
/*}*/

/*.formContainer::-webkit-scrollbar-thumb {*/
/*  background: #5f1ec1;*/
/*  border-radius: 8px;*/
/*}*/

/*.formContainer::-webkit-scrollbar-thumb:hover {*/
/*  background: #555;*/
/*}*/

/* Custom scrollbar styling for the react-select menu */
