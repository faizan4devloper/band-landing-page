/* Define default light theme styles */
:root {
  --card-title-light: rgba(23, 23, 25, 1);
  --back-button-light-bg: rgba(230, 235, 245, 1);
  --back-button-light-hover: rgba(95, 30, 193, 1);
}

/* Define dark theme styles */
[data-theme='dark'] {
  --card-title-light: rgba(255, 255, 255, 1);
  --back-button-light-bg: rgba(60, 60, 60, 1);
  --back-button-light-hover: rgba(200, 200, 200, 1);
}

.sideBarPage {
  /* existing styles */
  position: fixed;
  margin-top: 70px;
  margin-left: 45px;
  flex-direction: column;
  min-height: 100vh;
  overflow-y: auto;
  overscroll-behavior: contain;
  scroll-behavior: smooth;
}

.header2 {
  display: flex;
  align-items: center;
}

.cardTitle {
  font-size: 18px;
  color: var(--card-title-light); /* Use variable for color */
  font-weight: 600;
  margin-bottom: 10px;
  font-family: "Poppins", sans-serif;
}

.contentWrapper {
  display: flex;
  flex: 1;
}

.backButtonContainer {
  display: flex;
  align-items: center;
  padding: 10px;
}

.backButton {
  background-color: var(--back-button-light-bg); /* Use variable for background color */
  padding: 7px;
  margin-left: 20px;
  margin-bottom: 10px;
  border-radius: 4px;
  width: 32px;
  font-size: 14px;
  border: none;
  cursor: pointer;
  margin-right: 10px;
}

.backButton:hover {
  color: var(--back-button-light-hover); /* Use variable for hover color */
}
