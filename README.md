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

  const renderCustomThumbs = () => {
    return content.solutionFlow.map((image, index) => (
      <div className={styles.thumb} key={index}>
        <img src={image} alt={`Solution Flow Thumb ${index + 1}`} />
      </div>
    ));
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
        <div className={styles.carouselContainer}>
          <Carousel
            showArrows={false}
            showIndicators={false}
            renderThumbs={renderCustomThumbs}
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




.carouselContainer {
  display: flex;
  align-items: flex-start;
  margin-top: 20px;
}

.carousel {
  flex: 1;
}

.thumb {
  width: 80px;
  margin: 10px 0;
}

.thumb img {
  width: 100%;
  height: auto;
  cursor: pointer;
  border: 2px solid transparent;
  transition: border 0.3s;
}

.thumb img:hover {
  border: 2px solid #5f1ec1;
}

/* Other existing styles for .mainContent */
.mainContent {
  display: flex;
  flex-direction: column;
  width: 100%;
  padding: 0px 20px;
  background-color: #ffffff;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  height: calc(100vh - 100px); /* Adjust height as needed */
  overflow-y: auto; /* Enable vertical scroll */
}

.mainContent h2 {
  font-size: 20px;
  font-weight: 600;
  color: #5f1ec1;
  margin-bottom: 10px;
  margin-top: 0;
  border-bottom: 2px solid rgba(95, 30, 193, 0.8);
  padding-bottom: 5px;
}

.mainContent img {
  max-width: 90%;
  height: auto;
  display: block;
  margin: 20px auto;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  cursor: pointer;
  transition: transform 0.3s ease;
}

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
  top: 25px;
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

.benefits, .description, .demo, .architecture, .adoption, .solution {
  padding: 10px 15px;
  background-color: #f9f9f9;
  border-left: 4px solid rgba(95, 30, 193, 0.8);
  margin-bottom: 20px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  margin-bottom: 50px;
}

.benefits p, .description li {
  margin: 0;
  font-size: 12px;
}

.benefits h2, .description h2 {
  font-size: 20px;
  font-weight: 600;
  color: #5f1ec1;
  margin-bottom: 10px;
}

.adoptionTable {
  width: 100%;
  border-collapse: collapse;
  margin-top: 10px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  background-color: #fff;
}

.adoptionTable th, .adoptionTable td {
  border: 1px solid #ddd;
  font-size: 12px;
  padding: 10px;
  text-align: left;
  vertical-align: top;
}

.adoptionTable th {
  background-color: #5f1ec1;
  color: #fff;
  text-transform: capitalize;
  letter-spacing: 1px;
  font-weight: bold;
}

.adoptionTable tr:nth-child(even) {
  background-color: #f9f9f9;
}

.adoptionTable tr:hover {
  background-color: #f1f1f1;
}

.adoptionTable td:first-child {
  font-weight: bold;
  color: #5f1ec1;
}

.highlight {
  font-style: italic;
  color: #5f1ec1;
  font-weight: bold;
}
