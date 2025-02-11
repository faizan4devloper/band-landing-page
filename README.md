import React, { useState } from 'react';
import styles from "./ApproverComments.module.css";
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faPaperPlane, faCheckCircle, faChevronDown } from '@fortawesome/free-solid-svg-icons';

const ApproverComments = ({ approverComments, handleApproverCommentsChange }) => {
  const [isSubmitting, setIsSubmitting] = useState(false);
  const [isSubmitted, setIsSubmitted] = useState(false);
  const [decision, setDecision] = useState(""); // New state for dropdown

  const handleSubmit = () => {
    if (approverComments.trim() === '' || decision === "") {
      return;
    }

    setIsSubmitting(true);
    setTimeout(() => {
      setIsSubmitting(false);
      setIsSubmitted(true);

      setTimeout(() => {
        setIsSubmitted(false);
      }, 3000);
    }, 1500);
  };

  const handleReset = () => {
    setIsSubmitted(false);
    setDecision("");
  };

  const SuccessMessage = () => (
    <div className={styles.successMessageContainer}>
      <div className={styles.successMessage}>
        <FontAwesomeIcon icon={faCheckCircle} className={styles.successIcon} />
        <div>
          <h3>Comments Submitted Successfully!</h3>
          <p>Your remarks have been recorded and processed.</p>
        </div>
        <button className={styles.closeSuccessButton} onClick={handleReset}>Close</button>
      </div>
    </div>
  );

  if (isSubmitted) {
    return <SuccessMessage />;
  }

  return (
    <div className={styles.container}>
      <div className={styles.sectionWindow}>
        <div className={styles.header}>
          <h2>Approver Comments</h2>
          <div className={styles.headerUnderline}></div>
        </div>

        {/* Decision Dropdown */}
        <div className={styles.dropdownContainer}>
          <select 
            className={styles.dropdown} 
            value={decision} 
            onChange={(e) => setDecision(e.target.value)}
          >
            <option value="" disabled>Select Decision</option>
            <option value="Approve">Approve</option>
            <option value="Reject">Reject</option>
          </select>
          <FontAwesomeIcon icon={faChevronDown} className={styles.dropdownIcon} />
        </div>

        {/* Comment Box */}
        <textarea
          className={styles.approverCommentsTextarea}
          placeholder="Enter your final remarks here..."
          value={approverComments}
          onChange={handleApproverCommentsChange}
          rows={4}
          disabled={isSubmitting}
        />
        <div className={styles.characterCount}>{approverComments.length}/500</div>

        {/* Submit Button */}
        <div className={styles.submitButtonContainer}>
          <button 
            className={`${styles.submitButton} ${isSubmitting ? styles.submitting : ''} 
              ${approverComments.trim() === '' || decision === "" ? styles.disabled : ''}`}
            onClick={handleSubmit}
            disabled={isSubmitting || approverComments.trim() === '' || decision === ""}
          >
            {isSubmitting ? (
              <>
                <div className={styles.spinner}></div>
                Submitting...
              </>
            ) : (
              <>
                <FontAwesomeIcon icon={faPaperPlane} className={styles.submitIcon} />
                Submit
              </>
            )}
          </button>
        </div>
      </div>
    </div>
  );
};

export default ApproverComments;






/* Container */
.container {
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
}

/* Section Styling */
.sectionWindow {
  background: linear-gradient(135deg, #ffffff, #f9fafb);
  border: 1px solid #e1e4e8;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  border-radius: 12px;
  padding: 24px;
  transition: all 0.3s ease-in-out;
}

.sectionWindow:hover {
  transform: translateY(-5px);
  box-shadow: 0 10px 15px rgba(0, 0, 0, 0.15);
}

/* Header */
.header {
  margin-bottom: 20px;
}

.header h2 {
  font-size: 22px;
  color: #2c3e50;
  font-weight: 600;
}

.headerUnderline {
  height: 3px;
  width: 50px;
  background: linear-gradient(to right, #4a90e2, #50c878);
  margin-top: 8px;
  border-radius: 2px;
}

/* Dropdown */
.dropdownContainer {
  position: relative;
  margin-bottom: 16px;
}

.dropdown {
  width: 100%;
  padding: 12px;
  font-size: 16px;
  border: 1.5px solid #e1e4e8;
  border-radius: 8px;
  background-color: white;
  appearance: none;
  cursor: pointer;
  transition: border 0.3s ease;
}

.dropdown:focus {
  border-color: #4a90e2;
  box-shadow: 0 0 0 3px rgba(74, 144, 226, 0.2);
  outline: none;
}

.dropdownIcon {
  position: absolute;
  top: 50%;
  right: 12px;
  transform: translateY(-50%);
  color: #4a90e2;
  pointer-events: none;
}

/* Comment Box */
.approverCommentsTextarea {
  width: 100%;
  padding: 12px;
  border: 1.5px solid #e1e4e8;
  border-radius: 8px;
  font-size: 16px;
  line-height: 1.5;
  resize: vertical;
  min-height: 120px;
  max-height: 250px;
  transition: all 0.3s ease;
}

.approverCommentsTextarea:focus {
  outline: none;
  border-color: #4a90e2;
  box-shadow: 0 0 0 3px rgba(74, 144, 226, 0.2);
}

.characterCount {
  text-align: right;
  color: #6a737d;
  font-size: 12px;
  margin-top: 8px;
  opacity: 0.7;
}

/* Submit Button */
.submitButtonContainer {
  margin-top: 16px;
  display: flex;
  justify-content: flex-end;
}

.submitButton {
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 12px 24px;
  background: linear-gradient(135deg, #0d4fc0, #6a90d4);
  color: white;
  border: none;
  border-radius: 8px;
  font-size: 14px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.submitButton:hover:not(:disabled) {
  transform: translateY(-2px);
  box-shadow: 0 6px 8px rgba(0, 0, 0, 0.15);
}

.submitButton:disabled {
  opacity: 0.6;
  cursor: not-allowed;
  background: linear-gradient(to right, #a0a0a0, #b0b0b0);
}

.submitIcon {
  margin-right: 8px;
}

.submitting {
  cursor: wait;
}

/* Success Message */
.successMessageContainer {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  backdrop-filter: blur(5px);
}

.successMessage {
  background: white;
  border-radius: 16px;
  padding: 30px;
  max-width: 500px;
  width: 90%;
  text-align: center;
  box-shadow: 0 15px 35px rgba(0,0,0,0.1);
}

/* Spinner */
.spinner {
  border: 4px solid rgba(255, 255, 255, 0.3);
  border-top: 4px solid white;
  border-radius: 50%;
  width: 10px;
  height: 10px;
  animation: spin 1s linear infinite;
  margin-right: 10px;
}

@media (max-width: 600px) {
  .container {
    padding: 10px;
  }
}
