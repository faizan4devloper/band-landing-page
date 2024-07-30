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


.customCarousel .carousel .thumbs-wrapper {
  position: absolute;
  left: 0;
  top: 0;
  bottom: 0;
  width: 150px; /* Adjust width as needed */
  background-color: #fff;
  border-right: 1px solid #ddd;
  overflow-y: auto;
}

.customCarousel .carousel .thumbs {
  display: block;
}

.customCarousel .carousel .thumbs li {
  display: block;
  width: 100%;
  margin-bottom: 10px;
  padding: 5px;
  cursor: pointer;
}

.customCarousel .carousel .thumbs img {
  width: 100%;
  height: auto;
  display: block;
  border-radius: 4px;
  transition: transform 0.3s ease;
}

.customCarousel .carousel .thumbs .selected img {
  transform: scale(1.05);
  border: 2px solid #5f1ec1; /* Highlight selected thumbnail */
}

.customCarousel .carousel .carousel-slider {
  margin-left: 150px; /* Adjust margin to accommodate thumbnail width */
}
