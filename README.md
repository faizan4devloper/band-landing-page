add beutiful also style add the drop down of Approve/Reject

import React, { useState } from 'react';
import styles from "./ApproverComments.module.css";
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faPaperPlane, faCheck, faCheckCircle } from '@fortawesome/free-solid-svg-icons';

const ApproverComments = ({ 
  approverComments, 
  handleApproverCommentsChange 
}) => {
  const [isSubmitting, setIsSubmitting] = useState(false);
  const [isSubmitted, setIsSubmitted] = useState(false);

  const handleSubmit = () => {
    if (approverComments.trim() === '') {
      return;
    }

    // Simulate submission process
    setIsSubmitting(true);

    // Simulated async operation
    setTimeout(() => {
      setIsSubmitting(false);
      setIsSubmitted(true);

      // Reset submission state after 3 seconds
      setTimeout(() => {
        setIsSubmitted(false);
      }, 3000);
    }, 1500);
  };

  // Reset function
  const handleReset = () => {
    setIsSubmitted(false);
  };

  // Success Message Component
  const SuccessMessage = () => (
    <div className={styles.successMessageContainer}>
      <div className={styles.successMessage}>
        <FontAwesomeIcon 
          icon={faCheckCircle} 
          className={styles.successIcon} 
        />
        <div>
          <h3>Comments Submitted Successfully!</h3>
          <p>Your remarks have been recorded and processed.</p>
        </div>
        <button 
          className={styles.closeSuccessButton}
          onClick={handleReset}
        >
          Close
        </button>
      </div>
    </div>
  );

  // If submission is complete, show success message
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
        <textarea
          className={styles.approverCommentsTextarea}
          placeholder="Enter your final remarks here..."
          value={approverComments}
          onChange={handleApproverCommentsChange}
          rows={4}
          disabled={isSubmitting}
        />
        <div className={styles.characterCount}>
          {approverComments.length}/500
        </div>
        
        {/* Beautiful Submit Button */}
        <div className={styles.submitButtonContainer}>
          <button 
            className={`${styles.submitButton} ${
              isSubmitting ? styles.submitting : ''
            } ${
              approverComments.trim() === '' ? styles.disabled : ''
            }`}
            onClick={handleSubmit}
            disabled={isSubmitting || approverComments.trim() === ''}
          >
            {isSubmitting ? (
              <>
                <div className={styles.spinner}></div>
                Submitting...
              </>
            ) : (
              <>
                <FontAwesomeIcon 
                  icon={faPaperPlane} 
                  className={styles.submitIcon} 
                />
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



/*.container {*/
/*  max-width: 600px;*/
/*  margin: 0 auto;*/
/*  padding: 20px;*/
/*}*/

.sectionWindow {
  background: linear-gradient(135deg, #ffffff, #f9fafb);
  border: 1px solid #e1e4e8;
  box-shadow: 
    0 4px 6px rgba(0, 0, 0, 0.1),
    0 1px 3px rgba(0, 0, 0, 0.08);
  border-radius: 12px;
  padding: 24px;
  transition: all 0.3s ease-in-out;
}

.sectionWindow:hover {
  transform: translateY(-5px);
  box-shadow: 
    0 10px 15px rgba(0, 0, 0, 0.15),
    0 3px 6px rgba(0, 0, 0, 0.12);
}

.header {
  margin-bottom: 20px;
}

.header h2 {
  margin: 0;
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

.approverCommentsTextarea {
  width: 100%;
  /*padding: 12px;*/
  border: 1.5px solid #e1e4e8;
  border-radius: 8px;
  font-size: 16px;
  line-height: 1.5;
  resize: vertical;
  min-height: 120px;
  max-height: 250px;
  transition: all 0.3s ease;
  font-family: 'Inter', 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
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
  /*gap: 10px;*/
}

.submitButton:hover:not(:disabled) {
  transform: translateY(-2px);
  box-shadow: 0 6px 8px rgba(0, 0, 0, 0.15);
  background: linear-gradient(135deg, #0d4fc0, #6a90d4);
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
  z-index: 1000;
  backdrop-filter: blur(5px);
  animation: fadeIn 0.3s ease-out;
}


.successMessage {
  background: white;
  border-radius: 16px;
  padding: 30px;
  max-width: 500px;
  width: 90%;
  text-align: center;
  box-shadow: 
    0 15px 35px rgba(0,0,0,0.1),
    0 5px 15px rgba(0,0,0,0.08);
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 20px;
  position: relative;
  animation: popIn 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
}



.successIcon {
  color: #4CAF50;
  font-size: 80px;
  margin-bottom: 20px;
  animation: bounce 0.5s ease;
}


.successMessage h3 {
  margin: 0;
  color: #2c3e50;
  font-size: 24px;
}

.successMessage p {
  color: #7f8c8d;
  margin: 10px 0 20px;
}

.closeSuccessButton {
  background: linear-gradient(135deg, #0d4fc0, #6a90d4);
  color: white;
  border: none;
  padding: 12px 24px;
  border-radius: 8px;
  font-size: 14px;
  cursor: pointer;
  transition: all 0.3s ease;
}

.closeSuccessButton:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 6px rgba(0,0,0,0.1);
}

/* Spinner for Submission */
.spinner {
  border: 4px solid rgba(255, 255, 255, 0.3);
  border-top: 4px solid white;
  border-radius: 50%;
  width: 10px;
  height: 10px;
  animation: spin 1s linear infinite;
  margin-right: 10px;
}




/* Responsive Adjustments */
@media (max-width: 600px) {
  .container {
    padding: 10px;
  }

  .sectionWindow {
    padding: 16px;
  }
}
