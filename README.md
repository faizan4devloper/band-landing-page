/* Light theme */
:root {
  --search-input-bg-light: #fff;
  --search-input-border-light: #d3d3d3;
  --search-input-placeholder-light: #888;
  --search-input-focus-border-light: rgba(95, 30, 193, 1);
  --search-icon-color-light: #888;
}

/* Dark theme */
[data-theme='dark'] {
  --search-input-bg-dark: #333;
  --search-input-border-dark: #555;
  --search-input-placeholder-dark: #aaa;
  --search-input-focus-border-dark: rgba(173, 216, 230, 1); /* Lighter border for dark theme */
  --search-icon-color-dark: #aaa;
}

.searchBar {
  margin: 20px 0;
  display: flex;
  justify-content: center;
  position: absolute;
  bottom: 95%;
  left: 65%;
  align-items: center;
}

.inputWrapper {
  position: relative;
  width: 100%;
  max-width: 400px;
}

.searchInput {
  width: 70%;
  padding: 10px 16px 10px 40px;
  border: 1px solid var(--search-input-border-light); /* Default light theme border */
  border-radius: 50px;
  font-size: 16px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  background-color: var(--search-input-bg-light); /* Default light theme background */
  outline: none;
  transition: all 0.3s ease;
}

.searchInput::placeholder {
  color: var(--search-input-placeholder-light); /* Default light theme placeholder color */
  font-size: 12px;
  opacity: 0.7;
}

.searchInput:focus {
  border-color: var(--search-input-focus-border-light); /* Default light theme focus border */
  box-shadow: 0 4px 8px var(--search-input-focus-border-light);
  background-color: var(--search-input-bg-light); /* Default light theme focus background */
  transform: scale(1.02);
}

.searchIcon {
  position: absolute;
  left: 16px;
  top: 50%;
  transform: translateY(-50%);
  color: var(--search-icon-color-light); /* Default light theme icon color */
  pointer-events: none;
}

/* Dark theme styles */
[data-theme='dark'] .searchInput {
  border-color: var(--search-input-border-dark);
  background-color: var(--search-input-bg-dark);
  color: #fff; /* Text color in dark mode */
}

[data-theme='dark'] .searchInput::placeholder {
  color: var(--search-input-placeholder-dark);
}

[data-theme='dark'] .searchInput:focus {
  border-color: var(--search-input-focus-border-dark);
  box-shadow: 0 4px 8px var(--search-input-focus-border-dark);
}

[data-theme='dark'] .searchIcon {
  color: var(--search-icon-color-dark);
}
