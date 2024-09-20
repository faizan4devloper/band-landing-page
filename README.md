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
    /* Full viewport height */
    /*background-image: url('../../assets/images/welcome-background.jpg');*/
    /* Add a background image */
    background-size: cover;
    background-position: center;
    color: #fff;
    /* White text for better contrast */
    text-align: center;
    padding: 2rem;
    animation: fadeIn 1.5s ease-out;
    /* Apply fade-in animation */
}

.content {
    background: rgba(0, 0, 0, 0.6);
    /* Semi-transparent background for contrast */
    padding: 2rem;
    border-radius: 8px;
    /* Rounded corners */
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.3);
    /* Subtle shadow */
    animation: fadeIn 2s ease-out;
    /* Apply fade-in animation */
}


/* Title and Subtitle Styling */
.title {
    font-size: 3rem; /* Large title */
    font-weight: bold;
    margin-bottom: 0.5rem; /* Smaller margin for closer spacing */
    color: #ffffff; /* White text for contrast */
    text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.6); /* Subtle shadow for readability */
    line-height: 1.2; /* Tight line-height for title */
    letter-spacing: 1px; /* Slightly spaced letters for a modern look */
}

.subtitle {
    font-size: 1.2rem; /* Subtitle font size */
    line-height: 1.6; /* Line-height for readability */
    margin: 0 0 2rem 0; /* Spacing below subtitle */
    color: #e2e2e2; /* Slightly lighter color for contrast */
    text-shadow: 1px 1px 3px rgba(0, 0, 0, 0.4); /* Subtle shadow for readability */
    font-weight: 300; /* Light font-weight for a more refined look */
}


/* Button Styling */
.startButton {
    padding: 0.55rem 1rem;
    font-size: 1rem;
    font-weight: bold;
    color: #fff;
    background-color: #5f1ebe;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    transition: background-color 0.3s ease, transform 0.3s ease, box-shadow 0.3s ease;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
    transition: .5s ease;
    animation: pulsate 2s infinite;
    /* Apply pulsate animation */
}

.startButton:hover {
    background-color: #e2d9fb;
    /* Darker green on hover */
    transform: scale(1.1);
    /* Slightly larger button */
    box-shadow: 0 8px 16px rgba(0, 0, 0, 0.3); /* Enhanced shadow */

    color: #5f1ebe;
    animation: none;
    /* Stop pulsating animation on hover */
}

/* Button Active State */
.startButton:active {
    transform: scale(1); /* Reset size to normal */
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2); /* Original shadow */
}
