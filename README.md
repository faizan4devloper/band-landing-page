.mainContent {
  display: flex;
  flex-direction: column;
  width: 100%;
  padding: 0px 20px;
  background-color: #ffffff;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  height: calc(100vh - 100px); /* Adjust height as needed */
  overflow-y: auto; /* Enable vertical scroll */
  position: relative; /* Ensures the loader is centered */
}

.imageLoadingContainer {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100%; /* Takes full height of the parent container */
  position: absolute; /* Positions the loader absolutely within the mainContent */
  top: 0; /* Ensures the loader is centered vertically */
  left: 0; /* Ensures the loader is centered horizontally */
  width: 100%; /* Takes full width of the parent container */
}

.imageLoadingCaption {
  font-size: 12px;
  font-weight: bold;
  color: #5931d4;
  margin-bottom: 10px;
  animation: pulse 2s infinite;
}



const renderCarousel = (images) => (
  <div className={styles.carouselContainer}>
    <div className={styles.customThumbs}>
      {images.map((image, index) => (
        <div
          key={index}
          className={`${styles.customThumbContainer} ${currentSlide === index ? styles.selected : ""}`}
          onClick={() => setCurrentSlide(index)}
          title="Click to Enlarge"
        >
          <img
            src={image}
            alt={`Thumbnail ${index + 1}`}
            className={styles.customThumb}
            loading="lazy" /* Add lazy loading attribute */
          />
        </div>
      ))}
    </div>
    <Carousel
      showArrows={false}
      showIndicators={false}
      showThumbs={false}
      showStatus={false}
      selectedItem={currentSlide}
      onChange={(index) => setCurrentSlide(index)}
      className={styles.customCarousel}
    >
      {images.map((image, index) => (
        <div
          key={index}
          onClick={() => toggleMaximize(image)} // Toggle maximization on image click
        >
          <img src={image} alt={`Slide ${index + 1}`} title="Click to Enlarge" loading="lazy" /> {/* Add lazy loading attribute */}
        </div>
      ))}
    </Carousel>
  </div>
);
