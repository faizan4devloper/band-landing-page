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
  font-family: 'Nunito', 'Roboto', -apple-system, BlinkMacSystemFont, 'Segoe UI',
    'Oxygen', 'Ubuntu', 'Cantarell', 'Fira Sans', 'Droid Sans', 'Helvetica Neue', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  overflow-x: hidden;
  /* Prevent horizontal scrolling */
  overflow-y: auto;
  /* Enable vertical scrolling */
  background: linear-gradient(135deg, #0B3D91 0%, #F2F2F2 100%);
}

/* Hide scrollbar for Webkit browsers (Chrome, Safari, Edge) */
body::-webkit-scrollbar {
  width: 0;
  /* Hide scrollbar width */
}

/* Hide scrollbar for all elements in Webkit browsers */
*::-webkit-scrollbar {
  width: 0;
  height: 0;
}

/* Hide scrollbar for Firefox */
* {
  scrollbar-width: none;
  /* Firefox */
}

/* Ensure the root element uses 100% of the viewport */
#root {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}


<link href="https://fonts.googleapis.com/css2?family=Nunito:wght@400;700&family=Roboto:wght@400;500&display=swap" rel="stylesheet">
