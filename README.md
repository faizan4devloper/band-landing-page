import React, { useState } from "react";
import { Carousel } from "react-responsive-carousel";
import "react-responsive-carousel/lib/styles/carousel.min.css"; // Import carousel styles
import styles from "./MainContent.module.css";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faTimes } from "@fortawesome/free-solid-svg-icons";
import Video from "./Video";

const MainContent = ({ activeTab, content }) => {
  const [maximizedImage, setMaximizedImage] = useState(null);
  const [selectedIndex, setSelectedIndex] = useState(0);

  const toggleMaximize = (imageSrc) => {
    setMaximizedImage(maximizedImage === imageSrc ? null : imageSrc);
  };

  if (!content || !content.description) {
    return <div className={styles.mainContent}>Description not available</div>;
  }

  const handleThumbClick = (index) => {
    setSelectedIndex(index);
  };

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
        <div className={styles.carouselWrapper}>
          <div className={styles.thumbnails}>
            {content.solutionFlow.map((image, index) => (
              <img
                key={index}
                src={image}
                alt={`Thumbnail ${index + 1}`}
                className={`${styles.thumb} ${selectedIndex === index ? styles.selected : ""}`}
                onClick={() => handleThumbClick(index)}
              />
            ))}
          </div>
          <Carousel
            showArrows={false}
            showIndicators={false}
            selectedItem={selectedIndex}
            onChange={(index) => setSelectedIndex(index)}
            className={styles.carousel}
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






/* Carousel container styles */
.carouselWrapper {
  display: flex;
  align-items: center;
}

/* Main carousel styles */
.carousel {
  flex: 1;
}

/* Thumbnails container styles */
.thumbnails {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-right: 10px; /* Space between thumbnails and the main carousel */
}

/* Thumbnails styles */
.thumb {
  cursor: pointer;
  margin-bottom: 10px; /* Space between thumbnails */
}

/* Selected thumbnail styles */
.selected {
  border: 2px solid #5f1ec1; /* Border for the selected thumbnail */
}

/* Optional: Add any additional styles you need for the thumbnails */
