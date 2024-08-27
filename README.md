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

  // Toggles the maximization of the image when clicked
  const toggleMaximize = (imageSrc) => {
    const isMaximized = maximizedImage === imageSrc;
    setMaximizedImage(isMaximized ? null : imageSrc);
  };

  // Adds a mousemove event listener to track the cursor position when an image is maximized
  useEffect(() => {
    if (maximizedImage) {
      const handleMouseMove = (e) => {
        setLaserPos({ x: e.clientX, y: e.clientY });
      };
      document.addEventListener("mousemove", handleMouseMove);
      return () => {
        document.removeEventListener("mousemove", handleMouseMove);
      };
    }
  }, [maximizedImage]);

  // Renders a carousel with custom thumbnails
  const renderCarousel = (images) => (
    <div className={styles.carouselOverlayContainer}>
      <Carousel
        showArrows={true}
        showIndicators={false}
        showThumbs={false}
        showStatus={false}
        selectedItem={currentSlide}
        onChange={(index) => setCurrentSlide(index)}
        className={styles.customCarousel}
      >
        {images.map((image, index) => (
          <div key={index} onClick={() => toggleMaximize(image)}>
            <img
              src={image}
              alt={`Slide ${index + 1}`}
              className={styles.carouselImage}
              title="Click to Enlarge"
              loading="lazy"
            />
          </div>
        ))}
      </Carousel>
      <button
        className={`${styles.carouselOverlayNavButton} ${styles.prev}`}
        onClick={() => setCurrentSlide((prev) => (prev === 0 ? images.length - 1 : prev - 1))}
      >
        &lt;
      </button>
      <button
        className={`${styles.carouselOverlayNavButton} ${styles.next}`}
        onClick={() => setCurrentSlide((prev) => (prev === images.length - 1 ? 0 : prev + 1))}
      >
        &gt;
      </button>
    </div>
  );

  // Renders either a single image or a carousel based on the content provided
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

  // Renders either the images or a loader while content is being fetched
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

  // Returns early if content is not available       
  if (!content) {
    return (
      <div className={styles.mainContent}>Content not available</div>
    );
  }

  // Maps the activeTab to the corresponding content section
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
    <div className={`${styles.mainContent} ${maximizedImage ? styles.laserCursorEnabled : ""}`}>
      {contentMap[activeTab] || <div>Content not available</div>}
      {maximizedImage && (
        <div className={styles.overlay} onClick={() => setMaximizedImage(null)}>
          <FontAwesomeIcon
            icon={faTimes}
            className={styles.closeIcon}
            onClick={() => setMaximizedImage(null)}
          />
          {renderCarousel(content[activeTab])} {/* Show carousel in overlay */}
        </div>
      )}
      {maximizedImage && (
        <div
          className={styles.laserCursor}
          style={{ top: `${laserPos.y}px`, left: `${laserPos.x}px` }}
        />
      )}
    </div>
  );
};

export default MainContent;





/* Overlay styling */
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
}

/* Carousel container inside overlay */
.carouselOverlayContainer {
  position: relative;
  width: 80%;
  height: 80%;
  max-width: 1200px;
  max-height: 800px;
}

/* Carousel navigation buttons inside overlay */
.carouselOverlayNavButton {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  background: rgba(0, 0, 0, 0.7);
  border: none;
  color: #ffffff;
  padding: 10px;
  cursor: pointer;
  z-index: 1300;
}

.carouselOverlayNavButton.prev {
  left: 10px;
}

.carouselOverlayNavButton.next {
  right: 10px;
}
