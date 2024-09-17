import React from 'react';
import { useNavigate } from 'react-router-dom';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faFile } from '@fortawesome/free-solid-svg-icons';
import styles from './WelcomeScreen.module.css';

const WelcomeScreen = () => {
    const navigate = useNavigate(); // Hook to navigate programmatically

    const handleStartClick = () => {
        navigate('/home'); // Navigate to the home screen
    };

    return (
        <div className={styles.welcomeScreen}>
            <div className={styles.content}>
                <h1 className={styles.title}>ClaimAssist</h1>
                <p className={styles.subtitle}>
                    Empower your claim process to minimize Claim Denial and maximize reimbursements
                </p>
                <button className={styles.startButton} onClick={handleStartClick}>
                    Start Claim Assist <FontAwesomeIcon icon={faFile} />
                </button>
            </div>
        </div>
    );
};

export default WelcomeScreen;





.welcomeScreen {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh; /* Full viewport height */
    background-image: url('../../assets/images/welcome-background.jpg'); /* Add a background image */
    background-size: cover;
    background-position: center;
    color: #fff; /* White text for better contrast */
    text-align: center;
    padding: 2rem;
}

.content {
    background: rgba(0, 0, 0, 0.6); /* Semi-transparent background for contrast */
    padding: 2rem;
    border-radius: 8px; /* Rounded corners */
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.3); /* Subtle shadow */
}

.title {
    font-size: 3rem; /* Large title */
    font-weight: bold;
    margin-bottom: 1rem;
}

.subtitle {
    font-size: 1.5rem; /* Subtitle font size */
    line-height: 1.4;
    margin: 0 0 2rem 0;
}

/* Button Styling */
.startButton {
    padding: 0.75rem 2rem;
    font-size: 1.2rem;
    color: #5f1ebe;
    background-color: #e2d9fb; /* Green background */
    border: none;
    border-radius: 5px;
    cursor: pointer;
    transition: background-color 0.3s ease;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
}

.startButton:hover {
    background-color: #5f1ebe; /* Darker green on hover */
    color: #fff;
}

.startButton:active {
    background-color: #1e7e34; /* Even darker green on click */
}
