import React, { useState } from "react";
import axios from "axios";
import Select from "react-select";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faStar, faTimes } from "@fortawesome/free-solid-svg-icons";
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
        <FontAwesomeIcon icon={faTimes} />
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
  margin-top: 12px;
  font-size: 14px;
  width: 78%;
}

.successMessage {
  text-align: center;
  color: #5f1ec1;
  font-size: 16px;
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
