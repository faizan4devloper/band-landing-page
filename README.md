import React, { useState, useEffect } from "react";
import { Carousel } from "react-responsive-carousel";
import "react-responsive-carousel/lib/styles/carousel.min.css"; // Import carousel styles
import styles from "./MainContent.module.css";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faTimes } from "@fortawesome/free-solid-svg-icons";
import Video from "./Video";
import BeatLoader from "react-spinners/BeatLoader";

const MainContent = ({ activeTab, content }) => {
  const [maximizedImage, setMaximizedImage] = useState(null);
  const [currentSlide, setCurrentSlide] = useState(0);
  const [laserPos, setLaserPos] = useState({ x: 0, y: 0 });
  const [isImageMaximized, setIsImageMaximized] = useState(false);

  const toggleMaximize = (imageSrc) => {
    const isMaximized = maximizedImage === imageSrc;
    setMaximizedImage(isMaximized ? null : imageSrc);
    setIsImageMaximized(!isMaximized);
  };

  const handleMouseMove = (e) => {
    if (isImageMaximized) {
      setLaserPos({ x: e.clientX, y: e.clientY });
    }
  };

  useEffect(() => {
    document.addEventListener("mousemove", handleMouseMove);
    return () => {
      document.removeEventListener("mousemove", handleMouseMove);
    };
  }, [isImageMaximized]);

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

  const renderContent = (content) => {
    if (!content || content.length === 0) {
      return <BeatLoader color="#5931d4" size={8} />;
    }

    if (typeof content === "string") {
      return <div>{content}</div>;
    }

    return content.length > 1
      ? renderCarousel(content)
      : (
        <img
          src={content[0]}
          alt="Single Image"
          className={maximizedImage === content[0] ? styles.maximized : ""}
          onClick={() => toggleMaximize(content[0])}
          title="Click To Enlarge"
        />
      );
  };

  const renderImageOrCarousel = (images) => {
    if (!images || images.length === 0) {
      return (
        <div className={styles.imageLoadingContainer}>
          <p className={styles.imageLoadingCaption}>Processing, please wait</p>
          <BeatLoader color="#5931d4" size={8} />
        </div>
      );
    }

    return renderContent(images);
  };

  if (!content) {
    return (
      <div className={styles.mainContent}>Content not available</div>
    );
  }

  const contentMap = {
    description: (
      <div className={styles.description}>
        {renderImageOrCarousel(content.description)}
      </div>
    ),
    solutionFlow: (
      <div className={styles.solution}>
        {renderImageOrCarousel(content.solutionFlow)}
      </div>
    ),
    demo: (
      <div className={styles.demo}>
        <Video src={content.demo} />
      </div>
    ),
    techArchitecture: (
      <div className={styles.architecture}>
        {renderImageOrCarousel(content.techArchitecture)}
      </div>
    ),
    benefits: (
      <div className={styles.benefits}>
        {renderImageOrCarousel(content.benefits)}
      </div>
    ),
    adoption: (
      <div className={styles.adoption}>
        {renderImageOrCarousel(content.adoption)}
      </div>
    ),
  };

  return (
    <div className={`${styles.mainContent} ${isImageMaximized ? styles.laserCursorEnabled : ""}`}>
      {contentMap[activeTab] || <div>Content not available</div>}
      {maximizedImage && (
        <div className={styles.overlay} onClick={() => setMaximizedImage(null)}>
          <FontAwesomeIcon
            icon={faTimes}
            className={styles.closeIcon}
            onClick={() => setMaximizedImage(null)}
          />
          <img
            src={maximizedImage}
            alt="Maximized view"
            className={styles.maximizedImage}
          />
        </div>
      )}
      <div
        className={styles.laserCursor}
        style={{ top: `${laserPos.y}px`, left: `${laserPos.x}px` }}
      />
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
   cursor: none; /* Hide default cursor */
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  height: calc(100vh - 100px); /* Adjust height as needed */
  overflow-y: auto; /* Enable vertical scroll */
  min-height: 300px;
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
  margin: 10px auto;
  cursor: pointer;
  transition: transform 0.3s ease;
  z-index: 1100;
}

.maximized {
  max-width: 100%;
  max-height: 100%;
  width: 100%;
  height: 100%;
  object-fit: contain;
  margin: auto;
  display: block;
  transition: transform 0.3s ease;
  z-index: 1200;
}

.maximizedImage {
  width: 77%;
  height: 100%;
  object-fit: contain;
    z-index: 1200;

}

.closeIcon {
  position: absolute;
  top: 10px;
  right: 100px;
  font-size: 25px;
  color: #ffffff;
  cursor: pointer;
  z-index: 1300;
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
  z-index: 1200;
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
}

.customThumbContainer {
  cursor: pointer;
}

.customThumb {
  width: 100px; /* Adjust size as needed */
  height: 100px; /* Adjust size as needed */
  object-fit: cover;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  border: 2px solid transparent;
  transition: border-color 0.3s;
}

.customThumb:hover,
.selected .customThumb {
  border-color: #5f1ec1;
}

.customCarousel {
  flex: 1;
  cursor: pointer;
}


.imageLoadingContainer {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 20px;
  min-height: 340px;
  min-width: 610px;
}

.imageLoadingCaption {
  font-size: 12px;
  font-weight: bold;
  color: #5931d4;
  margin-bottom: 10px;
  animation: pulse 2s infinite;
}

@keyframes pulse {
  0% {
    transform: scale(1);
  }
  50% {
    transform: scale(1.1);
  }
  100% {
    transform: scale(1);
  }
}




.laserCursorEnabled .laserCursor {
  display: block; /* Ensure the laser cursor is visible when enabled */
}

.laserCursor {
  position: fixed;
  width: 20px; /* Adjust size as needed */
  height: 20px; /* Adjust size as needed */
  background-color: red; /* Red color for the laser cursor */
  border-radius: 50%; /* Make it a circle */
  pointer-events: none;
  display: none; /* Hide cursor by default */
  z-index: 1500;
}
