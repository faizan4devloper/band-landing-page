/* Remove default margins and paddings */
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

/* Set global styles for body */
body {
    overflow-x: hidden; /* Prevent horizontal scrolling */
    font-family: Arial, sans-serif; /* Set a base font */
    line-height: 1.6; /* Improve text readability */
}

/* Ensure full viewport height and no overflow */
html, body {
    width: 100%;
    height: 100%;
}

/* Set the container to take full width */
.container {
    max-width: 100%;
    overflow-x: hidden;
}





.welcomeScreen {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh; /* Full viewport height */
    background-image: url('../../assets/images/welcome-background.jpg');
    background-size: cover;
    background-position: center;
    color: #fff;
    text-align: center;
    padding: 2rem;
    overflow: hidden; /* Prevent overflow */
}

.content {
    max-width: 90%; /* Responsive width */
    background: rgba(0, 0, 0, 0.6);
    padding: 2rem;
    border-radius: 8px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.3);
}

.title {
    font-size: 2.5rem; /* Responsive font size */
}

.subtitle {
    font-size: 1.2rem;
}

/* Button Styling */
.startButton {
    padding: 0.75rem 2rem;
    font-size: 1rem;
}

@media (max-width: 768px) {
    .title {
        font-size: 2rem; /* Smaller font size on small screens */
    }

    .subtitle {
        font-size: 1rem;
    }

    .startButton {
        font-size: 0.9rem;
        padding: 0.5rem 1.5rem;
    }
}
