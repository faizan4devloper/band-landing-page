/* Fade-in animation for the welcome screen */
@keyframes fadeIn {
    0% {
        opacity: 0;
        transform: translateY(20px);
    }
    100% {
        opacity: 1;
        transform: translateY(0);
    }
}

/* Pulsating animation for the start button */
@keyframes pulsate {
    0% {
        transform: scale(1);
        box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
    }
    50% {
        transform: scale(1.05);
        box-shadow: 0 6px 12px rgba(0, 0, 0, 0.3);
    }
    100% {
        transform: scale(1);
        box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
    }
}

/* Welcome Screen */
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
    animation: fadeIn 1.5s ease-out; /* Apply fade-in animation */
}

.content {
    background: rgba(0, 0, 0, 0.6); /* Semi-transparent background for contrast */
    padding: 2rem;
    border-radius: 8px; /* Rounded corners */
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.3); /* Subtle shadow */
    animation: fadeIn 2s ease-out; /* Apply fade-in animation */
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
    animation: pulsate 2s infinite; /* Apply pulsate animation */
}

.startButton:hover {
    background-color: #5f1ebe; /* Darker green on hover */
    color: #fff;
    animation: none; /* Stop pulsating animation on hover */
}

.startButton:active {
    background-color: #1e7e34; /* Even darker green on click */
}
