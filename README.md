/* Theme Toggle Switch */
.themeToggleButton {
  position: relative;
  margin-right: 6px;
  width: 45px;
  height: 21px;
  background-color: var(--button-background-color);
  border-radius: 30px;
  cursor: pointer;
  display: flex;
  align-items: center;
  padding: 2px;
  transition: background-color 0.3s ease;
}

.themeToggleButton:hover {
  background-color: var(--button-hover-color);
}

.toggleCircle {
  position: absolute;
  top: 2px;
  left: 2px;
  width: 17px; /* Adjusted size to fit well */
  height: 17px; /* Adjusted size to fit well */
  background-color: var(--toggle-circle);
  border-radius: 50%;
  transition: transform 0.3s ease;
}

.themeToggleButton.light .toggleCircle {
  transform: translateX(0);
}

.themeToggleButton.dark .toggleCircle {
  transform: translateX(24px); /* Adjusted for 45px width */
}

.toggleIcon {
  position: absolute;
  width: 16px; /* Adjusted size */
  height: 16px; /* Adjusted size */
  transition: opacity 0.3s ease;
}

.sunIcon {
  left: 5px;
  color: yellow; /* Direct color assignment for SVG */
  opacity: 0;
}

.moonIcon {
  right: 5px;
  opacity: 1;
}

.themeToggleButton.light .sunIcon {
  opacity: 1; /* Show sun icon in light mode */
}

.themeToggleButton.light .moonIcon {
  opacity: 0; /* Hide moon icon in light mode */
}

.themeToggleButton.dark .sunIcon {
  opacity: 0; /* Hide sun icon in dark mode */
}

.themeToggleButton.dark .moonIcon {
  opacity: 1; /* Show moon icon in dark mode */
}
