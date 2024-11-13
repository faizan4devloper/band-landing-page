/* MainContent.module.css */

/* Container for the loader to center it */
.spinnerContainer {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%); /* Centering the loader */
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh; /* Takes full viewport height */
  width: 100%;   /* Takes full width */
  background-color: rgba(255, 255, 255, 0.8); /* Semi-transparent background */
  z-index: 999; /* Ensures the loader is on top of other content */
}

/* You can adjust the size of the loader itself if needed */
.loaderWrapper {
  display: flex;
  justify-content: center;
  align-items: center;
}

.clipLoader {
  border-color: #36d7b7 !important;  /* Sets loader color */
}
