.carouselItem {
  position: relative;
  left: 50%;
  transform: translateX(-50%);
  height: 280px;
  margin: 70px 0;
  overflow: hidden;
  width: 85%;
  border-radius: 10px;
}

.carouselImage {
  width: 100%;
  height: 100%;
  object-fit: cover; /* Ensure the image covers the entire area */
  object-position: center; /* Center the image */
}
.carouselImage6 {
  width: 100%;
  height: 100%;
  object-fit: fill; /* Ensure the image covers the entire area */
  object-position: center; /* Center the image */
}

.carouselOverlay {
  position: absolute;
  border-radius: 6px;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: linear-gradient(90deg, #6f36cd 0%, rgba(31, 119, 246, 0.73) 100%);
}
.carouselOverlay6 {
  position: absolute;
  border-radius: 6px;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: linear-gradient(90deg, #6f36cd 0%, rgba(31, 119, 246, 0.73) 0%);
}

.carouselCaption {
  position: absolute;
  top: 50%; /* Position the caption vertically in the middle */
  left: 50%; /* Position the caption horizontally in the middle */
  transform: translate(-50%, -50%); /* Center the caption both vertically and horizontally */
  text-align: center; /* Center the text within the caption */
  color: white;
  font-family: "Poppins", sans-serif; /* Apply Poppins font family */
  z-index: 1;
}

.carouselCaption h2 {
  display: flex;
  font-weight: 600;
  font-size: 38px;
  margin: 0;
  padding: 15px;
}

.carouselCaption span {
  align-items: center;
  font-size: 16px;
  font-weight: 600;
  border-left: 3px solid rgba(255, 255, 255, 1);
  padding: 18px 10px;
  margin-left: 15px;
}

.customIndicator .carousel .control-dots .dot {
  background-color: rgba(95, 30, 193, 1); /* Custom color for inactive indicators */
}

.customIndicator .carousel .control-dots .dot.selected {
  background-color: #6F36CD; /* Custom color for active indicator */
}



