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
    height: 75vh;
    background-size: cover;
    background-position: center;
    color: #ffffff;
    text-align: center;
    padding: 2rem;
    animation: fadeIn 1.5s ease-out;
}

.content {
    background: rgba(11, 61, 145, 0.8); /* Darkened version of the background for better contrast */
    padding: 2rem;
    border-radius: 8px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.3);
    animation: fadeIn 2s ease-out;
}

/* Title and Subtitle Styling */
.title {
    font-size: 3rem;
    font-weight: bold;
    margin-bottom: 0.5rem;
    color: #F2F2F2; /* Light gray to match the gradient */
    text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.6);
    line-height: 1.2;
    letter-spacing: 1px;
}

.subtitle {
    font-size: 1.2rem;
    line-height: 1.6;
    margin: 0 0 2rem 0;
    color: #c9d6e3; /* A slightly lighter gray for contrast */
    text-shadow: 1px 1px 3px rgba(0, 0, 0, 0.4);
    font-weight: 300;
}

/* Button Styling */
.startButton {
    padding: 0.55rem 1rem;
    font-size: 1rem;
    font-weight: bold;
    color: #0B3D91; /* Matches the darker part of the background */
    background-color: #F2F2F2; /* Matches the lighter part of the gradient */
    border: none;
    border-radius: 5px;
    cursor: pointer;
    transition: background-color 0.3s ease, transform 0.3s ease, box-shadow 0.3s ease;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
    animation: pulsate 2s infinite;
}

.startButton:hover {
    background-color: #D9E2EC; /* A lighter version for hover */
    transform: scale(1.1);
    box-shadow: 0 8px 16px rgba(0, 0, 0, 0.3);
    color: #0B3D91;
    animation: none;
}

.startButton:active {
    transform: scale(1);
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
}
