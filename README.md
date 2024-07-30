import React, { useState } from "react";
import { Carousel } from "react-responsive-carousel";
import "react-responsive-carousel/lib/styles/carousel.min.css"; // Required for Carousel functionality
import styles from "./MainContent.module.css"; // Import CSS module
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faTimes } from "@fortawesome/free-solid-svg-icons";
import Video from "./Video";

const MainContent = ({ activeTab, content }) => {
  const [maximizedImage, setMaximizedImage] = useState(null);

  const toggleMaximize = (imageSrc) => {
    setMaximizedImage(maximizedImage === imageSrc ? null : imageSrc);
  };

  if (!content || !content.description) {
    return <div className={styles.mainContent}>Description not available</div>;
  }

  const contentMap = {
    description: (
      <div className={styles.description}>
        <img
          src={content.descriptionFlow}
          alt="Description Flow"
          className={maximizedImage === content.descriptionFlow ? styles.maximized : ""}
          onClick={() => toggleMaximize(content.descriptionFlow)}
        />
      </div>
    ),
    solutionFlow: (
      <div className={styles.solution}>
        <Carousel
          showArrows={false}
          showIndicators={false}
          renderThumbs={() =>
            content.solutionFlow.map((image, index) => (
              <img
                key={index}
                src={image}
                alt={`Thumbnail ${index + 1}`}
                className={`${styles.customThumb} ${maximizedImage === image ? styles.selected : ""}`}
                onClick={() => toggleMaximize(image)}
              />
            ))
          }
          className={styles.customCarousel} // Apply custom class for custom styling
        >
          {content.solutionFlow.map((image, index) => (
            <div key={index}>
              <img
                src={image}
                alt={`Solution Flow ${index + 1}`}
                className={maximizedImage === image ? styles.maximized : ""}
                onClick={() => toggleMaximize(image)}
              />
            </div>
          ))}
        </Carousel>
      </div>
    ),
    // Other tabs and corresponding content...
  };

  return (
    <div className={styles.mainContent}>
      {contentMap[activeTab] || <div>Content not available</div>}
      {maximizedImage && (
        <div className={styles.overlay} onClick={() => setMaximizedImage(null)}>
          <FontAwesomeIcon icon={faTimes} className={styles.closeIcon} onClick={() => setMaximizedImage(null)} />
          <img src={maximizedImage} alt="Maximized view" className={styles.maximized} />
        </div>
      )}
    </div>
  );
};

export default MainContent;



/* MainContent.module.css */

/* Custom styles for the carousel container */
.customCarousel .carousel .thumbs-wrapper {
  position: absolute;
  top: 50%;
  left: 0;
  transform: translateY(-50%);
  width: 100px;
  padding: 0; /* No padding around the thumbs */
  display: flex;
  flex-direction: column;
  align-items: center;
}

.customCarousel .carousel .thumb {
  width: 80px;
  height: 80px;
  margin: 10px 0;
  border: 2px solid transparent;
  border-radius: 4px;
  cursor: pointer;
  transition: border-color 0.3s ease;
}

.customCarousel .carousel .thumb.selected {
  border-color: #5f1ec1 !important;
}

/* Ensure the main carousel slider is offset to the right */
.customCarousel .carousel .slider-wrapper {
  margin-left: 100px; /* Adjust this value if needed */
}

/* Other styles for maximized image and overlays */
.maximized {
  max-width: 80%;
  max-height: 80%;
  margin: auto;
  display: block;
}

.closeIcon {
  position: absolute;
  top: 40px;
  right: 20px;
  font-size: 25px;
  color: #ffffff;
  cursor: pointer;
  z-index: 1001;
}
.closeIcon:hover {
  color: #808080;
}

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
  z-index: 1000;
  cursor: pointer;
}

/* Styles for other sections like .description, .benefits, etc. */
.benefits, .description, .demo, .architecture, .adoption, .solution {
  padding: 10px 15px;
  background-color: #f9f9f9;
  border-left: 4px solid rgba(95, 30, 193, 0.8);
  margin-bottom: 20px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.mainContent {
  display: flex;
  flex-direction: column;
  width: 100%;
  padding: 0px 20px;
  background-color: #ffffff;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  height: calc(100vh - 100px);
  overflow-y: auto;
}


