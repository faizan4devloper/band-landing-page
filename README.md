/* Default Light Theme */
:root {
  --background-color: #ffffff;
  --background-gradient: linear-gradient(to bottom, #ffffff, #f0f0f0);
  --box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  --scrollbar-track: #f1f1f1;
  --scrollbar-thumb: #5f1ec1;
  --scrollbar-thumb-hover: #3d1299;
  --border-color: rgba(95, 30, 193, 0.8);
  --image-border: transparent;
  --section-background: #f9f9f9;
  --section-border: rgba(95, 30, 193, 0.8);
  --highlight-color: #5f1ec1;
  --laser-cursor-color: #ff0000;
  --laser-cursor-shadow: 4px 3px 20px rgba(255, 0, 0, 54.8), 0 0 11px rgba(255, 0, 0, 202.6);
  --nav-button-color: #ffffff;
  --nav-button-hover: #808080;
}

/* Dark Theme */
[data-theme="dark"] {
  --background-color: #1a1a2e;
  --background-gradient: linear-gradient(to bottom, #1a1a2e, #16213e);
  --box-shadow: 0 2px 4px rgba(0, 0, 0, 0.3);
  --scrollbar-track: #333333;
  --scrollbar-thumb: #9d66f5;
  --scrollbar-thumb-hover: #c1a1f2;
  --border-color: rgba(95, 30, 193, 0.8);
  --image-border: transparent;
  --section-background: #333;
  --section-border: rgba(95, 30, 193, 0.8);
  --highlight-color: #9d66f5;
  --laser-cursor-color: #ff0000;
  --laser-cursor-shadow: 4px 3px 20px rgba(255, 0, 0, 54.8), 0 0 11px rgba(255, 0, 0, 202.6);
  --nav-button-color: #ffffff;
  --nav-button-hover: #808080;
}

/* Main content styling */
.mainContent {
  display: flex;
  flex-direction: column;
  width: 100%;
  padding: 0px 20px;
  background-color: var(--background-color);
  box-shadow: var(--box-shadow);
  height: calc(100vh - 100px);
  overflow-y: auto;
  min-height: 300px;
}

/* Custom Scrollbar Styling */
.mainContent::-webkit-scrollbar {
  width: 12px;
}

.mainContent::-webkit-scrollbar-track {
  background: var(--scrollbar-track);
}

.mainContent::-webkit-scrollbar-thumb {
  background-color: var(--scrollbar-thumb);
  border-radius: 20px;
  border: 3px solid var(--scrollbar-track);
}

.mainContent::-webkit-scrollbar-thumb:hover {
  background-color: var(--scrollbar-thumb-hover);
}

/* Image styling before maximization */
.mainContent img {
  max-width: 90%;
  height: auto;
  display: block;
  margin: 10px auto;
  transition: transform 0.3s ease;
  z-index: 1100;
  cursor: pointer;
}

/* Image styling after maximization */
.maximized,
.maximizedImage {
  max-width: 100%;
  max-height: 100%;
  width: 100%;
  height: 100%;
  object-fit: contain;
  margin: auto;
  display: block;
  transition: transform 0.3s ease;
  cursor: none; /* Hide default cursor after maximization */
  z-index: 1200;
}

/* Ensures the cursor is hidden over the maximized image */
.maximizedImage {
  width: 77%;
  height: 100%;
  object-fit: contain;
  cursor: none !important; /* Hide default cursor after maximization */
  z-index: 1200;
}

/* General cursor hiding for maximized images and overlays */
.maximized,
.maximizedImage,
.overlay {
  cursor: none;
}

/* Close icon styling */
.closeIcon {
  position: absolute;
  top: 10px;
  right: 125px;
  font-size: 18px;
  color: var(--nav-button-color);
  cursor: pointer;
  z-index: 1300;
}

.closeIcon:hover {
  color: var(--nav-button-hover);
}

/* Overlay styling */
.overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.9);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1200;
}

/* Section styling */
.benefits,
.description,
.demo,
.architecture,
.adoption,
.solution {
  padding: 10px 15px;
  background-color: var(--section-background);
  border-left: 4px solid var(--section-border);
  margin-bottom: 20px;
  box-shadow: var(--box-shadow);
  margin-bottom: 50px;
}

.highlight {
  font-style: italic;
  color: var(--highlight-color);
  font-weight: bold;
}

/* Carousel and thumb styling */
.carouselContainer {
  display: flex;
}

.customThumbs {
  display: flex;
  flex-direction: column;
}

.customThumbContainer {
  cursor: pointer;
}

.customThumb {
  width: 100px;
  height: 100px;
  object-fit: cover;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  border: 2px solid var(--image-border);
  transition: border-color 0.3s;
}

.customThumb:hover,
.selected .customThumb {
  border-color: var(--highlight-color);
}

.customCarousel {
  flex: 1;
  cursor: pointer;
}

/* Image loading container */
.imageLoadingContainer {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 20px;
  min-height: 340px;
  min-width: 610px;
}

.imageLoadingCaption {
  font-size: 12px;
  font-weight: bold;
  color: var(--highlight-color);
  margin-bottom: 10px;
  animation: pulse 2s infinite;
}

@keyframes pulse {
  0% {
    transform: scale(1);
  }
  50% {
    transform: scale(1.1);
  }
  100% {
    transform: scale(1);
  }
}

/* Laser cursor enabled */
.laserCursorEnabled {
  display: block;
  cursor: none !important;
}

.laserCursor {
  position: fixed;
  width: 10px; /* Adjust size to resemble a laser pointer */
  height: 10px; /* Adjust size to resemble a laser pointer */
  background-color: var(--laser-cursor-color); /* Red color for the laser cursor */
  border-radius: 50%; /* Make it a circle */
  pointer-events: none;
  z-index: 1500;
  box-shadow: var(--laser-cursor-shadow);
  animation: pulseLaser 1.5s infinite;
}

.laserCursor::before {
  content: "";
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 4px; /* Adjust the size of the white center */
  height: 4px; /* Adjust the size of the white center */
  background-color: #ffffff; /* White color for the center */
  border-radius: 50%; /* Make it a circle */
}

/* Navigation button styling */
.navButton {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  font-size: 24px;
  color: var(--nav-button-color);
  cursor: pointer;
  z-index: 1300;
}

.navButton:hover {
  color: var(--nav-button-hover);
}

.navButton:first-child {
  left: 60px;
}

.navButton:last-child {
  right: 60px;
}
