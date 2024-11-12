import React, { useState } from 'react';
import Sidebar from './Sidebar';
import MainContent from './MainContent';
import Chatbot from './../../../components/ChatBot/Chatbot';

import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faArrowLeft } from '@fortawesome/free-solid-svg-icons'; // Import FontAwesome left arrow icon

import styles from './EducationPage.module.css';

const EducationPage = () => {
  const [activeTopic, setActiveTopic] = useState(1);
  
  const handleBackClick = () => {
    // Handle the back button functionality here (navigate to previous page or reset the state)
    console.log('Back button clicked');
  };

  return (
    <div className={styles.educationPage}>
      <Sidebar setActiveTopic={setActiveTopic} />
      <div className={styles.mainContentWithBack}>
        {/* Back button */}
        <button className={styles.backButton} onClick={handleBackClick}>
          <FontAwesomeIcon icon={faArrowLeft} /> Back
        </button>
        <MainContent activeTopic={activeTopic} />
      </div>
      <Chatbot />
    </div>
  );
};

export default EducationPage;



.educationPage {
  display: flex;
  padding: 0 60px;
  height: 100vh;
  overflow: hidden;
}

.mainContentWithBack {
  flex: 1;
  position: relative; /* For positioning the back button */
}

.backButton {
  position: absolute;
  top: 20px;
  left: 20px;
  padding: 10px 20px;
  background-color: #5f1ec1; /* Purple background */
  color: white;
  font-size: 16px;
  border: none;
  border-radius: 5px;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: background-color 0.3s ease, transform 0.2s ease;
}

.backButton:hover {
  background-color: #4a139c; /* Darker purple on hover */
  transform: scale(1.05); /* Slight zoom effect */
}

.backButton svg {
  margin-right: 8px; /* Space between the icon and the text */
}








import { useNavigate } from 'react-router-dom';

const EducationPage = () => {
  const navigate = useNavigate();
  
  const handleBackClick = () => {
    navigate(-1); // Navigate back to the previous page
  };
  
  return (
    // JSX as above
  );
};

