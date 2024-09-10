:root {
  --primary-color: #6f36cd;
  --secondary-color: #1f77f6;
  --card-bg: #fff;
  --card-text-color: #000;
  --card-hover-bg: rgba(0, 0, 0, 0.1);
  --popup-bg: rgba(255, 255, 255, 0.9);
  --card-shadow: 0px 3px 4px rgba(92, 85, 85, 0.5); /* Default shadow */
}

[data-theme="dark"] {
  --primary-color: #9d66f5;
  --secondary-color: #c1a1f2;
  --card-bg: #333;
  --card-text-color: #fff;
  --card-hover-bg: rgba(255, 255, 255, 0.1);
  --popup-bg: rgba(0, 0, 0, 0.9);
  --card-shadow: 0px 3px 6px rgba(0, 0, 0, 0.7); /* Dark theme shadow */
}

body {
  background-color: var(--card-bg);
  color: var(--card-text-color);
  font-family: "Poppins", sans-serif;
}

.card {
  width: 160px;
  overflow: hidden;
  height: 160px;
  border-radius: 12px;
  background-color: var(--card-bg);
  background-size: cover;
  background-position: center;
  cursor: pointer;
  position: relative;
  margin-bottom: 15px;
  transition: transform 0.6s ease, background-color 0.3s ease, box-shadow 0.3s ease;
  box-shadow: var(--card-shadow);
}

.card:hover {
  transform: translate(0, -10px);
  background-color: var(--card-hover-bg);
}

.big {
  width: 190px;
  height: 190px;
  transform: scale(1.1);
}

.cardTitle {
  position: absolute;
  font-size: 12px;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 60px;
  background: linear-gradient(180deg, rgba(0, 0, 0, 0) 0%, #000000 100%);
  border-radius: 12px;
  color: white;
  padding: 18px;
  box-sizing: border-box;
  text-align: center;
}

.cardContent {
  position: absolute;
  font-size: 10px;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 140px;
  background: linear-gradient(180deg, rgba(0, 0, 0, 0) 0%, #000000 100%);
  border-radius: 12px;
  color: white;
  padding: 18px;
  box-sizing: border-box;
  transform: translateY(100%);
  transition: transform 0.3s ease, background 0.3s ease;
}

[data-theme="dark"] .cardContent {
  background: linear-gradient(90deg, var(--primary-color) 0%, var(--secondary-color) 100%);
}

.cardContent h3 {
  color: #eeecec;
}

.cardContent.slide-up {
  transform: translateY(0);
}

.cardContent::before {
  content: "";
  position: absolute;
  top: 100%;
  left: 0;
  width: 100%;
  height: 100%;
  background: linear-gradient(0deg, var(--primary-color) 0%, var(--secondary-color) 100%);
  border-radius: 12px;
  z-index: -1;
  transition: top 0.3s ease;
}

.card:hover .cardContent::before {
  top: 0;
}

.arrow {
  cursor: pointer;
  position: absolute;
  display: flex;
  bottom: 10px;
  right: 10px;
  font-size: 14px;
  width: 16px;
  height: 16px;
  padding: 5px;
  border-radius: 50px;
  border: 2px solid var(--card-text-color);
  color: var(--card-text-color);
  transition: width 0.3s ease, background 0.5s ease, color 0.5s ease;
}

.arrow:hover {
  width: 80px;
  background-color: var(--card-text-color);
  color: var(--secondary-color);
}

.cardPopup {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background: var(--popup-bg);
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3);
  text-align: center;
  width: 80%;
  z-index: 10;
  animation: fadeIn 0.3s ease-in-out;
  color: var(--card-text-color);
}

.closeButton {
  background: linear-gradient(90deg, var(--primary-color) 0%, var(--secondary-color) 100%);
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 5px;
  cursor: pointer;
}

.closeButton:hover {
  background-color: #0056b3;
}

@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translate(-50%, -45%);
  }
  to {
    opacity: 1;
    transform: translate(-50%, -50%);
  }
}

.theme-toggle-button {
  padding: 10px;
  background-color: var(--primary-color);
  border: none;
  border-radius: 5px;
  color: white;
  cursor: pointer;
  margin: 20px;
}

.theme-toggle-button:hover {
  background-color: var(--secondary-color);
}
