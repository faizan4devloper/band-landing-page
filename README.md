import React from 'react';
import { useNavigate } from 'react-router-dom';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faFileAlt } from '@fortawesome/free-solid-svg-icons';
import { motion } from 'framer-motion';
import styles from './WelcomeScreen.module.css';

const WelcomeScreen = () => {
    const navigate = useNavigate(); // Hook to navigate programmatically

    const handleStartClick = () => {
        navigate('/home'); // Navigate to the home screen
    };

    return (
        <motion.div
            className={styles.welcomeScreen}
            initial={{ opacity: 0 }}
            animate={{ opacity: 1 }}
            transition={{ duration: 1.5 }}
        >
            <motion.div
                className={styles.content}
                initial={{ y: 50, opacity: 0 }}
                animate={{ y: 0, opacity: 1 }}
                transition={{ duration: 2, delay: 0.5 }}
            >
                <motion.h1
                    className={styles.title}
                    initial={{ scale: 0.8, opacity: 0 }}
                    animate={{ scale: 1, opacity: 1 }}
                    transition={{ duration: 1, delay: 1 }}
                >
                    ClaimAssist
                </motion.h1>
                <motion.p
                    className={styles.subtitle}
                    initial={{ x: -50, opacity: 0 }}
                    animate={{ x: 0, opacity: 1 }}
                    transition={{ duration: 1, delay: 1.2 }}
                >
                    Empower your claim process to minimize Claim Denial and maximize reimbursements
                </motion.p>
                <motion.button
                    className={styles.startButton}
                    onClick={handleStartClick}
                    whileHover={{ scale: 1.05 }}
                    whileTap={{ scale: 0.95 }}
                >
                    <span className={styles.buttonText}>Start Claim Assist</span>
                    <FontAwesomeIcon icon={faFileAlt} className={styles.icon} />
                </motion.button>
            </motion.div>
            {/* Optional: Add decorative elements like floating shapes */}
            <div className={styles.decorativeShapes}>
                <span className={styles.shape}></span>
                <span className={styles.shape}></span>
                <span className={styles.shape}></span>
            </div>
        </motion.div>
    );
};

export default WelcomeScreen;


/* Import any necessary fonts */
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@300;400;700&display=swap');

/* Keyframes for background floating shapes */
@keyframes float {
    0% {
        transform: translateY(0) rotate(0deg);
    }
    50% {
        transform: translateY(-20px) rotate(10deg);
    }
    100% {
        transform: translateY(0) rotate(-10deg);
    }
}

/* Welcome Screen Container */
.welcomeScreen {
    position: relative;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    background: linear-gradient(135deg, #6e8efb, #a777e3);
    color: #fff;
    text-align: center;
    padding: 2rem;
    overflow: hidden;
    font-family: 'Roboto', sans-serif;
}

/* Content Box */
.content {
    background: rgba(0, 0, 0, 0.5);
    backdrop-filter: blur(10px);
    padding: 3rem 2rem;
    border-radius: 12px;
    box-shadow: 0 8px 32px rgba(0, 0, 0, 0.37);
    max-width: 600px;
    width: 100%;
    animation: fadeIn 2s ease-out;
}

/* Title Styling */
.title {
    font-size: 3rem;
    font-weight: 700;
    margin-bottom: 1rem;
    color: #ffffff;
    text-shadow: 2px 2px 6px rgba(0, 0, 0, 0.6);
    line-height: 1.2;
    letter-spacing: 1px;
}

/* Subtitle Styling */
.subtitle {
    font-size: 1.3rem;
    line-height: 1.6;
    margin-bottom: 2.5rem;
    color: #e0e0e0;
    text-shadow: 1px 1px 4px rgba(0, 0, 0, 0.4);
    font-weight: 300;
}

/* Start Button Styling */
.startButton {
    display: inline-flex;
    align-items: center;
    padding: 0.75rem 1.5rem;
    font-size: 1.1rem;
    font-weight: 600;
    color: #fff;
    background: linear-gradient(45deg, #ff6b6b, #f94d6a);
    border: none;
    border-radius: 50px;
    cursor: pointer;
    transition: background 0.3s ease, box-shadow 0.3s ease;
    box-shadow: 0 6px 12px rgba(0, 0, 0, 0.3);
    position: relative;
    overflow: hidden;
}

.startButton::before {
    content: '';
    position: absolute;
    top: -50%;
    left: -50%;
    width: 200%;
    height: 200%;
    background: rgba(255, 255, 255, 0.1);
    transform: rotate(45deg) scale(0);
    transition: transform 0.5s ease;
}

.startButton:hover::before {
    transform: rotate(45deg) scale(1);
}

.buttonText {
    margin-right: 0.5rem;
    z-index: 1;
}

/* Icon Styling */
.icon {
    transition: transform 0.3s ease;
}

.startButton:hover .icon {
    transform: translateX(5px);
}

/* Decorative Floating Shapes */
.decorativeShapes {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    pointer-events: none;
}

.shape {
    position: absolute;
    width: 100px;
    height: 100px;
    background: rgba(255, 255, 255, 0.15);
    border-radius: 50%;
    animation: float 6s ease-in-out infinite;
    opacity: 0.7;
}

.shape:nth-child(1) {
    top: 20%;
    left: 15%;
    animation-delay: 0s;
}

.shape:nth-child(2) {
    top: 60%;
    left: 70%;
    animation-delay: 2s;
    width: 150px;
    height: 150px;
}

.shape:nth-child(3) {
    top: 80%;
    left: 30%;
    animation-delay: 4s;
    width: 80px;
    height: 80px;
}

/* Responsive Design */
@media (max-width: 768px) {
    .content {
        padding: 2rem 1.5rem;
    }

    .title {
        font-size: 2.5rem;
    }

    .subtitle {
        font-size: 1.1rem;
    }

    .startButton {
        padding: 0.6rem 1.2rem;
        font-size: 1rem;
    }
}

/* Additional Animations */
@keyframes fadeIn {
    from {
        opacity: 0;
    }
    to {
        opacity: 1;
    }
}
