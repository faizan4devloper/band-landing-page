import React from 'react';
import { useNavigate } from 'react-router-dom';
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
                    Start Claim Assist
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
    color: #fff;
    background-color: #28a745; /* Green background */
    border: none;
    border-radius: 5px;
    cursor: pointer;
    transition: background-color 0.3s ease;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
}

.startButton:hover {
    background-color: #218838; /* Darker green on hover */
}

.startButton:active {
    background-color: #1e7e34; /* Even darker green on click */
}


import React from 'react';
import { BrowserRouter as Router, Routes, Route } from 'react-router-dom';
import Header from './components/Header/Header';
import Breadcrumbs from './components/Breadcrumbs/Breadcrumbs';
import WelcomeScreen from './components/WelcomeScreen/WelcomeScreen';
import HomeScreen from './components/HomeScreen/HomeScreen'; // Create a HomeScreen component

function App() {
    return (
        <Router>
            <div className="App">
                <Header />
                <Routes>
                    <Route path="/" element={<WelcomeScreen />} />
                    <Route path="/home" element={<HomeScreen />} />
                </Routes>
            </div>
        </Router>
    );
}

export default App;


import React from 'react';

const HomeScreen = () => {
    return (
        <div>
            <h1>Home Screen</h1>
            <p>Welcome to the ClaimAssist Home Screen!</p>
        </div>
    );
};

export default HomeScreen;
