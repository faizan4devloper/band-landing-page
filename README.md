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
  overflow-x: hidden;
  /* Prevent horizontal scrolling */
  overflow-y: auto;
  /* Enable vertical scrolling */
  /*background-image: url('./assets/images/welcome-background.jpg');*/
  /*background-size: cover;*/
  /*background: linear-gradient(135deg, #1B98E0 0%, #FF005C 100%);*/
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
  /* Chrome, Safari, Edge */
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
