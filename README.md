import React, { useState } from "react";
import { Carousel } from "react-responsive-carousel";
import "react-responsive-carousel/lib/styles/carousel.min.css"; // Import carousel styles
import styles from "./MainContent.module.css";
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
        <h2>Description</h2>
        <img
          src={content.descriptionFlow}
          alt="description Flow"
          className={maximizedImage === content.descriptionFlow ? styles.maximized : ""}
          onClick={() => toggleMaximize(content.descriptionFlow)}
        />
      </div>
    ),
    solutionFlow: (
      <div className={styles.solution}>
        <h2>Solution Flow</h2>
        <div className={styles.customCarousel}>
          <Carousel
            showArrows={false}
            showIndicators={false}
            showThumbs={true}
            thumbWidth={100} // Optional: Adjust thumbnail width
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
      </div>
    ),
    demo: (
      <div className={styles.demo}>
        <h2>Demo</h2>
        <Video src={content.demo} />
      </div>
    ),
    techArchitecture: (
      <div className={styles.architecture}>
        <h2>Technical Architecture</h2>
        <img
          src={content.techArchitecture}
          alt="Technical Architecture"
          className={maximizedImage === content.techArchitecture ? styles.maximized : ""}
          onClick={() => toggleMaximize(content.techArchitecture)}
        />
      </div>
    ),
    benefits: (
      <div className={styles.benefits}>
        <h2>Benefits</h2>
        <img
          src={content.benefitsFlow}
          alt="Benefits Flow"
          className={maximizedImage === content.benefitsFlow ? styles.maximized : ""}
          onClick={() => toggleMaximize(content.benefitsFlow)}
        />
      </div>
    ),
    adoption: (
      <div className={styles.adoption}>
        <h2>Solution Adoption</h2>
        <img
          src={content.adoptionFlow}
          alt="Adoption Flow"
          className={maximizedImage === content.adoptionFlow ? styles.maximized : ""}
          onClick={() => toggleMaximize(content.adoptionFlow)}
        />
      </div>
    ),
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







/* Wrapper for the carousel and thumbnails */
.custom-carousel .carousel .slider-wrapper {
  display: flex;
  flex-direction: row-reverse; /* This will place the main image on the right and thumbnails on the left */
}

.custom-carousel .carousel .thumbs-wrapper {
  width: 120px; /* Adjust the width as needed */
  order: -1; /* This will place the thumbnails before the main image */
  background-color: #f9f9f9; /* Background color for thumbnail area */
}

.custom-carousel .carousel .thumbs {
  display: flex;
  flex-direction: column;
  align-items: center; /* Center the thumbnails */
}

.custom-carousel .carousel .thumb {
  margin-bottom: 10px; /* Space between thumbnails */
  display: block;
  width: 100%; /* Make thumbnails take full width */
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1); /* Optional: Add shadow to thumbnails */
  border-radius: 5px; /* Optional: Rounded corners */
}

.custom-carousel .carousel .thumb.selected,
.custom-carousel .carousel .thumb:hover {
  border: 2px solid #5f1ec1; /* Border color for selected and hovered thumbnails */
}

.custom-carousel .carousel .thumb img {
  width: 100%;
  display: block;
  border-radius: 5px; /* Optional: Rounded corners for images */
}
