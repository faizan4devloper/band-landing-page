/* Apply box-sizing to all elements */
*,
*::before,
*::after {
    box-sizing: border-box;
}

/* Reset margin and padding */
body {
    margin: 0;
    padding: 0;
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', 'Roboto', 'Oxygen',
        'Ubuntu', 'Cantarell', 'Fira Sans', 'Droid Sans', 'Helvetica Neue', sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    overflow-x: hidden; /* Prevent horizontal scrolling */
}

/* Ensure the root element uses 100% of the viewport */
#root {
    min-height: 100vh;
    display: flex;
    flex-direction: column;
}
