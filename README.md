/* Button Styling */
.startButton {
    padding: 0.75rem 2rem;
    font-size: 1.2rem;
    color: #5f1ebe;
    background-color: #e2d9fb; /* Light background */
    border: none;
    border-radius: 5px;
    cursor: pointer;
    transition: background-color 0.3s ease, transform 0.3s ease, box-shadow 0.3s ease; /* Smooth transitions */
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2); /* Initial shadow */
    animation: pulsate 2s infinite; /* Pulsating animation */
}

/* Button Hover State */
.startButton:hover {
    background-color: #5f1ebe; /* Darker background on hover */
    color: #fff; /* White text on hover */
    transform: scale(1.1); /* Slightly larger button */
    box-shadow: 0 8px 16px rgba(0, 0, 0, 0.3); /* Enhanced shadow */
    animation: none; /* Stop pulsating animation on hover */
}

/* Button Active State */
.startButton:active {
    background-color: #1e7e34; /* Even darker background on click */
    transform: scale(1); /* Reset size to normal */
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2); /* Original shadow */
}
