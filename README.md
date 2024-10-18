/* global.css */

/* Define global variables for the color palette */
:root {
  --primary-dark-purple: rgb(58, 34, 102);
  --primary-tech-purple: rgb(58, 44, 120);
  --primary-mid-purple: rgb(132, 70, 165);
  --primary-light-purple: rgb(194, 144, 235);
  --primary-dark-blue: rgb(32, 34, 99);
  --primary-tech-blue: rgb(56, 120, 235);
  --primary-mid-blue: rgb(110, 180, 245);
  --primary-light-blue: rgb(164, 224, 255);
  --primary-tech-grey: rgb(200, 200, 200);
  
  --secondary-dark-coral: rgb(255, 69, 69);
  --secondary-coral: rgb(255, 127, 127);
  --secondary-light-coral: rgb(255, 184, 184);
  --secondary-dark-yellow: rgb(254, 194, 54);
  --secondary-yellow: rgb(255, 238, 80);
  --secondary-light-yellow: rgb(255, 255, 184);
  --secondary-dark-green: rgb(50, 205, 50);
  --secondary-green: rgb(144, 238, 144);
  --secondary-light-green: rgb(200, 250, 200);
  --secondary-dark-teal: rgb(0, 128, 128);
  --secondary-teal: rgb(72, 209, 204);
  --secondary-light-teal: rgb(175, 238, 238);

  --tertiary-bronze: rgb(205, 127, 50);
  --tertiary-cream: rgb(245, 245, 220);
  --tertiary-grey1: rgb(169, 169, 169);
  --tertiary-grey2: rgb(190, 190, 190);
  --tertiary-grey3: rgb(211, 211, 211);
  --tertiary-grey4: rgb(224, 224, 224);
  --tertiary-grey5: rgb(240, 240, 240);
}

/* Global styles to be applied across components */
body {
  margin: 0;
  font-family: 'Arial', sans-serif;
  background-color: var(--primary-light-blue);
  color: var(--primary-dark-blue);
}

/* Utility classes */
.text-center {
  text-align: center;
}

.flex {
  display: flex;
}

.flex-center {
  justify-content: center;
  align-items: center;
}

.w-full {
  width: 100%;
}

.h-screen {
  height: 100vh;
}

.bg-primary {
  background-color: var(--primary-dark-purple);
}

.bg-secondary {
  background-color: var(--secondary-coral);
}

.button-primary {
  background-color: var(--primary-dark-blue);
  color: white;
  padding: 0.75rem 1.5rem;
  border-radius: 0.5rem;
  cursor: pointer;
  transition: background-color 0.3s ease;
  font-weight: bold;
}

.button-primary:hover {
  background-color: var(--primary-tech-blue);
}

.button-secondary {
  background-color: var(--secondary-yellow);
  color: white;
  padding: 0.75rem 1.5rem;
  border-radius: 0.5rem;
  cursor: pointer;
  transition: background-color 0.3s ease;
  font-weight: bold;
}

.button-secondary:hover {
  background-color: var(--secondary-dark-yellow);
}

/* Input Fields */
.input-field {
  width: 100%;
  padding: 0.75rem;
  border: 2px solid var(--primary-tech-grey);
  border-radius: 0.5rem;
  font-size: 1rem;
  transition: border-color 0.3s ease;
}

.input-field:focus {
  border-color: var(--primary-tech-blue);
}
