/* Default Light Theme */
:root {
  --sidebar-bg: #ffffff;
  --sidebar-border: rgba(219, 197, 255, 1);
  --menu-item-bg-hover: rgba(230, 235, 245, 1);
  --menu-item-active-bg: linear-gradient(90deg, #6f36cd 0%, #1f77f6 100%);
  --menu-item-active-color: white;
  --icon-img-filter: brightness(0) invert(0);
  --text-color: #000000;
  --button-bg: rgba(230, 235, 245, 1);
  --button-hover-color: rgba(95, 30, 193, 1);
  --card-title-color: rgba(23, 23, 25, 1);
  --header-bg: #ffffff;
}

/* Dark Theme */
[data-theme="dark"] {
  --primary-color: #9d66f5;
  --secondary-color: #c1a1f2;
  --sidebar-bg: #2c2c2c;
  --sidebar-border: rgba(50, 50, 50, 1);
  --menu-item-bg-hover: rgba(70, 70, 70, 1);
  --menu-item-active-bg: linear-gradient(90deg, #3a2d7f 0%, #2d5d8f 100%);
  --menu-item-active-color: #ffffff;
  --icon-img-filter: brightness(0) invert(1);
  --text-color: #ffffff;
  --button-bg: #3a3a3a; /* Dark theme button background */
  --button-hover-color: #9d66f5; /* Dark theme button hover color */
  --card-title-color: #ffffff; /* Dark theme card title color */
  --header-bg: #2c2c2c;
}

/* Sidebar Page Styling */
.sideBarPage {
  position: fixed;
  margin-top: 70px;
  margin-left: 45px;
  flex-direction: column;
  min-height: 100vh;
  overflow-y: auto; /* Enable vertical scrolling */
  overscroll-behavior: contain; /* Prevent overscrolling */
  scroll-behavior: smooth; /* Enable smooth scrolling */
  background-color: var(--header-bg);
}

/* Header Styling */
.header2 {
  display: flex;
  align-items: center;
  color: var(--text-color); /* Text color based on theme */
}

/* Card Title Styling */
.cardTitle {
  font-size: 18px;
  color: var(--card-title-color); /* Card title color based on theme */
  font-weight: 600;
  margin-bottom: 10px;
  font-family: "Poppins", sans-serif;
}

/* Content Wrapper Styling */
.contentWrapper {
  display: flex;
  flex: 1;
}

/* Back Button Container Styling */
.backButtonContainer {
  display: flex;
  align-items: center;
  padding: 10px;
}

/* Back Button Styling */
.backButton {
  background-color: var(--button-bg); /* Button background based on theme */
  padding: 7px;
  margin-left: 20px;
  margin-bottom: 10px;
  border-radius: 4px;
  width: 32px;
  font-size: 14px;
  border: none;
  cursor: pointer;
  margin-right: 10px;
  color: var(--text-color); /* Text color based on theme */
}

.backButton:hover {
  color: var(--button-hover-color); /* Change button color on hover based on theme */
}
