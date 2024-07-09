.carouselItem {
  position: relative;
  left: 50%;
  transform: translateX(-50%);
  height: 260px; /* Adjust height as needed */
  width: 85%;
  margin: 0 auto; /* Center the carouselItem horizontally */
  border-radius: 10px;
}

.carouselImage {
  width: 100%;
  height: 100%;
  object-fit: cover; /* Ensure the image covers the entire area */
  object-position: center; /* Center the image */
  display: block; /* Ensure the image behaves as a block element */
}

.carouselOverlay6 {
  position: absolute;
  border-radius: 6px;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: linear-gradient(90deg, #6f36cd 0%, rgba(31, 119, 246, 0.73) 100%);
}
