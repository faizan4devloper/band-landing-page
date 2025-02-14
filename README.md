import React, { useState, useEffect } from 'react';
import axios from 'axios';
import styles from './VerificationDB.module.css';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { 
  faCheckCircle, 
  faTimesCircle, 
  faExclamationTriangle 
} from '@fortawesome/free-solid-svg-icons';

const VerificationDB = ({ 
  recNum,  // Add recNum as a prop
  psid  // Add policyNumber as a prop
}) => {
  const [verificationData, setVerificationData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  // API Endpoint
  const API_ENDPOINT = 'claimdbverify';

  // Fetch Verification Data
  const fetchVerificationData = async () => {
    try {
      setLoading(true);
      const payload = {
        claimid: recNum,
        recnumber: psid,  // You might want to pass this as a prop or use a default
        policynumber: "L2065777"
      };
      
      console.log('Payloads:', payload)

      const response = await axios.post(API_ENDPOINT, payload);
      console.log('VerificationDB:', response)
      setVerificationData(response.data);
      setLoading(false);
    } catch (error) {
      console.error('Verification DB Error:', error);
      setError(error.message || 'Failed to fetch verification data');
      setLoading(false);
    }
  };

  useEffect(() => {
    // Only fetch if recNum and policyNumber are provided
    if (recNum && psid) {
      fetchVerificationData();
    }
  }, [recNum, psid]);

  // Parse and categorize messages
  const parseMessages = (body) => {
    // Safely check if body exists and is a string
    if (!body || typeof body !== 'string') {
      return {
        successMessages: [],
        unsuccessMessages: []
      };
    }

    try {
      // More robust parsing method
      const successMatch = body.includes('Successful Messages:') 
        ? body.split('Successful Messages:')[1].split('Unsuccessful Messages:')[0]
        : '';
      
      const unsuccessMatch = body.includes('Unsuccessful Messages:')
        ? body.split('Unsuccessful Messages:')[1]
        : '';

      return {
        successMessages: successMatch
          .trim()
          .split('\n')
          .filter(msg => msg.trim() !== ''),
        unsuccessMessages: unsuccessMatch
          .trim()
          .split('\n')
          .filter(msg => msg.trim() !== '')
      };
    } catch (error) {
      console.error('Message parsing error:', error);
      return {
        successMessages: [],
        unsuccessMessages: []
      };
    }
  };

  // Render Message Section
const renderMessageSection = (messages, type) => {
  const icons = {
    success: faCheckCircle,
    error: faTimesCircle,
    warning: faExclamationTriangle
  };

  const messageStyles = {
    success: styles.successMessage,
    error: styles.errorMessage,
    warning: styles.warningMessage
  };

  return (
    <div className={messageStyles[`${type}Container`]}>
      <h4>
        <FontAwesomeIcon 
          icon={icons[type]} 
          className={styles[`${type}Icon`]} 
        />
        {type === 'success' ? 'Successful Verifications' : 'Issues Identified'}
      </h4>
      <ol className={styles.numberedMessageList}>
        {messages.map((message, index) => (
          <li 
            key={index} 
            className={`${styles.messageItem} ${messageStyles[type]}`}
          >
            <span className={styles.messageText}>{message}</span>
          </li>
        ))}
      </ol>
    </div>
  );
};
  // Detailed Rendering
  const renderVerificationContent = () => {
    // Loading State
    if (loading) return (
      <div className={styles.loadingContainer}>
        <div className={styles.spinner}></div>
        <p>Loading verification data...</p>
      </div>
    );

    // Error State
    if (error) return (
      <div className={styles.errorContainer}>
        <FontAwesomeIcon icon={faTimesCircle} className={styles.errorIcon} />
        <p>Error: {error}</p>
        <button onClick={fetchVerificationData}>Retry</button>
      </div>
    );

    // No Data State
    if (!verificationData || !verificationData.body) return (
      <div className={styles.noDataContainer}>
        <FontAwesomeIcon icon={faExclamationTriangle} className={styles.warningIcon} />
        <p>No verification data available</p>
      </div>
    );

    // Parse Messages
    const { successMessages, unsuccessMessages } = parseMessages(verificationData.body);

    return (
      <div className={styles.verificationContainer}>
       

        <div className={styles.messageContainer}>
          {successMessages.length > 0 && renderMessageSection(successMessages, 'success')}
          {unsuccessMessages.length > 0 && renderMessageSection(unsuccessMessages, 'error')}
        </div>
      </div>
    );
  };

  return (
    <div className={styles.verificationDBContainer}>
      {renderVerificationContent()}
    </div>
  );
};

export default VerificationDB;
