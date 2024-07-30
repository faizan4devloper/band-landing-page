import React, { useState } from "react";
import { Carousel } from "react-responsive-carousel";
import "react-responsive-carousel/lib/styles/carousel.min.css"; // Import carousel styles
import styles from "./MainContent.module.css";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faTimes } from "@fortawesome/free-solid-svg-icons";
import Video from "./Video";

const MainContent = ({ activeTab, content }) => {
  const [maximizedImage, setMaximizedImage] = useState(null);
  const [activeThumb, setActiveThumb] = useState(null);

  const toggleMaximize = (imageSrc) => {
    setMaximizedImage(maximizedImage === imageSrc ? null : imageSrc);
  };

  const handleThumbClick = (imageSrc, index) => {
    setActiveThumb(index);
    toggleMaximize(imageSrc);
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
      <div className={styles.carouselContainer}>
        <div className={styles.customThumbs}>
          {content.solutionFlow.map((image, index) => (
            <div
              key={index}
              className={styles.customThumbContainer}
              onClick={() => handleThumbClick(image, index)}
            >
              <img
                src={image}
                alt={`Thumbnail ${index + 1}`}
                className={`${styles.customThumb} ${activeThumb === index ? styles.active : ""}`}
              />
              <div className={styles.thumbNumber}>{index + 1}</div>
            </div>
          ))}
        </div>
        <Carousel
          showArrows={false}
          showIndicators={false}
          className={styles.customCarousel}
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
    demo: (
      <div className={styles.demo}>
        <Video src={content.demo} />
      </div>
    ),
    techArchitecture: (
      <div className={styles.architecture}>
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




.carouselContainer {
  display: flex;
}

.customThumbs {
  display: flex;
  flex-direction: column;
  margin-right: 20px; /* Adjust margin for spacing */
}

.customThumbContainer {
  position: relative;
  cursor: pointer;
  margin-bottom: 10px; /* Space between thumbnails */
}

.customThumb {
  width: 100px; /* Adjust size as needed */
  height: 100px; /* Adjust size as needed */
  object-fit: cover;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  border: 2px solid transparent;
  transition: border-color 0.3s, background-color 0.3s;
}

.customThumb:hover,
.selected .customThumb {
  border-color: #5f1ec1; /* Highlight border color */
}

.customThumb.active {
  border-color: #5f1ec1; /* Active thumbnail border color */
  background-color: rgba(95, 30, 193, 0.3); /* Semi-transparent background color */
}

.thumbNumber {
  position: absolute;
  top: 5px;
  left: 5px;
  background-color: rgba(0, 0, 0, 0.6); /* Semi-transparent background for number */
  color: #fff; /* Text color */
  padding: 3px 6px;
  border-radius: 50%;
  font-size: 14px;
  font-weight: bold;
}

.customCarousel {
  flex: 1;
}
