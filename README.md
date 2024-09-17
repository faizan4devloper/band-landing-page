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
  scrollbar-width: thin;
  scrollbar-color: #888 #f1f1f1;
}

/* Ensure the root element uses 100% of the viewport */
#root {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

/* Webkit Browsers */
::-webkit-scrollbar {
  width: 8px;
}

::-webkit-scrollbar-track {
  background: linear-gradient(180deg, #e0e0e0, #f1f1f1);
  border-radius: 10px;
}

::-webkit-scrollbar-thumb {
  background: linear-gradient(180deg, #888, #555);
  /* Gradient handle */
  border-radius: 10px;
  box-shadow: inset 0 0 5px rgba(0, 0, 0, 0.2);
  /* Inner shadow */
}

::-webkit-scrollbar-thumb:hover {
  background: linear-gradient(180deg, #555, #333);
  /* Darker gradient on hover */
}
