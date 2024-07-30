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
        <div className={styles.carouselContainer}>
          <div className={styles.customThumbs}>
            {content.solutionFlow.map((image, index) => (
              <div
                key={index}
                className={`${styles.customThumbContainer} ${maximizedImage === image ? styles.selected : ""}`}
                onClick={() => toggleMaximize(image)}
              >
                <img src={image} alt={`Thumbnail ${index + 1}`} className={styles.customThumb} />
              </div>
            ))}
          </div>
          <Carousel showArrows={false} showIndicators={false} className={styles.customCarousel}>
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

/* Custom Scrollbar Styling */
.mainContent::-webkit-scrollbar {
  width: 12px; /* Width of the scrollbar */
}

.mainContent::-webkit-scrollbar-track {
  background: #f1f1f1; /* Track background color */
}

.mainContent::-webkit-scrollbar-thumb {
  background-color: #5f1ec1; /* Thumb color */
  border-radius: 20px; /* Rounded corners */
  border: 3px solid #f1f1f1; /* Border around the thumb */
}

.mainContent::-webkit-scrollbar-thumb:hover {
  background-color: #3d1299; /* Thumb color on hover */
}

.mainContent ul {
  list-style-type: disc;
  margin-left: 20px;
  padding-left: 20px;
}

.mainContent ul li {
  font-size: 12px;
  line-height: 1.6;
  color: #000;
  margin-bottom: 10px;
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

.benefits,
.description,
.demo,
.architecture,
.adoption,
.solution {
  padding: 10px 15px;
  background-color: #f9f9f9;
  border-left: 4px solid rgba(95, 30, 193, 0.8);
  margin-bottom: 20px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  margin-bottom: 50px;
}

.benefits p,
.description li {
  margin: 0;
  font-size: 12px;
}

.benefits h2,
.description h2 {
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

.adoptionTable th,
.adoptionTable td {
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

.carouselContainer {
  display: flex;
}

.customThumbs {
  display: flex;
  flex-direction: column;
  margin-right: 10px;
}

.customThumbContainer {
  margin-bottom: 5px;
  cursor: pointer;
}

.customThumb {
  width: 80px;
  height: 80px;
  object-fit: cover;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  border: 2px solid transparent;
  transition: border-color 0.3s;
}

.customThumb:hover,
.selected .customThumb {
  border-color: #5f1ec1;
}
