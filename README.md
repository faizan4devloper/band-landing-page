.carouselContainer {
  width: 100%;
  margin: 90px 0px 50px 0px;
}

.carouselItem {
  position: relative;
  left: 50%;
  transform: translateX(-50%);
  height: 280px;
  margin-bottom: 20px;
  overflow: hidden;
  width: 85%;
  border-radius: 10px;
  cursor: pointer;
}

.carouselImage {
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: center;
}

.carouselImage6 {
  width: 100%;
  height: 100%;
  object-fit: fill;
  object-position: center;
}

.carouselOverlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: linear-gradient(90deg, #6f36cd 0%, rgba(31, 119, 246, 0.73) 100%);
  border-radius: 6px;
}

.carouselCaption {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  text-align: center;
  color: white;
  font-family: "Poppins", sans-serif;
  z-index: 1;
}

.carouselCaption h2 {
  display: flex;
  font-weight: 600;
  font-size: 38px;
  margin: 0;
  padding: 15px;
  color: #fff;
}

.carouselCaption span {
  align-items: center;
  font-size: 16px;
  font-weight: 600;
  border-left: 3px solid rgba(255, 255, 255, 1);
  padding: 18px 10px;
  margin-left: 15px;
  color: #fff;
}

.carousel .slide {
  min-width: 100%;
  margin: 0;
  height: 352px !important;
  position: relative;
  text-align: center;
  overflow: hidden;
}

/* Custom Indicator Styles */
.indicatorsContainer {
  display: flex;
  justify-content: center;
  list-style: none;
  padding: 0;
  margin-top: 10px;
}

.dot {
  width: 12px;
  height: 12px;
  border-radius: 50%;
  display: inline-block;
  margin: 0 5px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.dot.selected {
  background-color: #6f36cd;
}
