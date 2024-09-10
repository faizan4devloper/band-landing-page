/* Default Light Theme */
:root {
  --primary-color: #5f1ec1;
  --secondary-color: rgba(15, 95, 220, 1);
  --background-color: #ffffff;
  --text-color: #808080;
  --scrollbar-color: rgba(15, 95, 220, 1);
  --scrollbar-background: #dcdcdc;
  --button-background-color: rgba(13, 85, 198, 0.1);
  --button-hover-color: #5f1ec1;
  --navbar-background: linear-gradient(to bottom, #ffffff, #f0f0f0);
  --navbar-border: rgba(219, 197, 255, 1);
  --modal-background: #ffffff;
  --overlay-background: rgba(0, 0, 0, 0.9);
}

/* Dark Theme */
[data-theme="dark"] {
  --primary-color: #9d66f5;
  --secondary-color: #c1a1f2;
  --background-color: #1a1a2e;
  --text-color: #ffffff;
  --scrollbar-color: #5f1ec1;
  --scrollbar-background: #333333;
  --button-background-color: rgba(95, 30, 193, 0.8);
  --button-hover-color: #c1a1f2;
  --navbar-background: linear-gradient(to bottom, #1a1a2e, #16213e);
  --navbar-border: rgba(219, 197, 255, 1);
  --modal-background: #333;
  --overlay-background: rgba(0, 0, 0, 0.9);
}

/* .navbarWrapper Styles */
.navbarWrapper {
  position: fixed;
  top: 0;
  left: 0;
  min-width: 100%;
  background: var(--navbar-background);
  border-bottom: 0.1px solid var(--navbar-border);
  padding: 8px 0px;
  transition: transform 0.5s ease-in-out;
}

.navbarWrapper.hide {
  transform: translateY(-100%);
}

.navbarWrapper.show {
  transform: translateY(0);
}

.navbarWrapper .header {
  display: flex;
  align-items: center;
  justify-content: space-around;
  margin: 5px -200px;
}

.logo img {
  max-height: 20px;
  cursor: pointer;
}

.right {
  display: flex;
  align-items: center;
}

.button {
  display: flex;
  align-items: center;
  background: linear-gradient(90deg, var(--primary-color) 0%, var(--secondary-color) 100%);
  color: white;
  border: none;
  padding: 8px 8px;
  margin: 0px 5px;
  font-size: 12px;
  border-radius: 5px;
  cursor: pointer;
  transition: .6s ease;
}

.button:hover {
  transform: translate(0, -5px);
}

.content {
  padding-top: 70px; /* Adjust the value to match the height of your header */
}

.logo img::after {
  content: attr(title);
  position: absolute;
  top: 100%;
  left: 50%;
  transform: translateX(-50%);
  background-color: #333;
  color: #fff;
  padding: 5px;
  border-radius: 4px;
  font-size: 12px;
  white-space: nowrap;
  opacity: 0;
  visibility: hidden;
  transition: opacity 0.3s, visibility 0.1s;
}

.logo img:hover::after {
  opacity: 1;
  visibility: visible;
}

@keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}

.modal {
  position: absolute;
  top: 50%;
  left: 50%;
  right: auto;
  bottom: auto;
  transform: translate(-50%, -50%);
  background: var(--modal-background);
  border-radius: 8px;
  padding: 20px;
  margin-top: 30px;
  max-width: 900px;
  width: 90%;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  animation: fadeIn 0.3s ease-out;
}

.overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: var(--overlay-background);
  animation: fadeIn 0.3s ease-out;
}

.button img {
  width: 13px;
  height: 13px;
}

.whiteImage {
  width: 28px;
  margin-left: 14px;
  cursor: pointer;
  transition: .6s ease;
}

.whiteImage:hover {
  transform: translate(0, -5px);
}

.feedbackbutton {
  background: transparent;
}

/* Theme Toggle Button */
.themeToggleButton {
  position: fixed;
  bottom: 20px;
  right: 20px;
  background-color: var(--button-background-color);
  border: none;
  color: var(--text-color);
  padding: 10px 20px;
  border-radius: 30px;
  cursor: pointer;
  display: flex;
  align-items: center;
  gap: 8px;
  transition: background-color 0.3s ease, transform 0.3s ease;
}

.themeToggleButton:hover {
  background-color: var(--button-hover-color);
  transform: scale(1.1);
}

.themeIcon {
  font-size: 16px;
}

.themeText {
  font-size: 14px;
  font-weight: 500;
}
