/// Renders either the images or a message if no images are available
const renderImageOrCarousel = (images) => {
  if (!images || images.length === 0) {
    return (
      <div className={styles.noImageContainer}>
        <p className={styles.noImageMessage}>No images available for this section.</p>
      </div>
    );
  }

  return renderContent(images);
};



.noImageContainer {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100%;
  text-align: center;
  padding: 20px;
  background-color: #f7f7f7; /* Light grey background */
  border-radius: 8px;
  border: 1px solid #e0e0e0; /* Subtle border */
}

.noImageMessage {
  font-size: 1.2rem;
  color: #666; /* Slightly muted text color */
  font-weight: 500;
  margin: 0;
}









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

  /// Toggles the maximization of the image when clicked
  const toggleMaximize = (imageSrc) => {
    const isMaximized = maximizedImage === imageSrc;
    setMaximizedImage(isMaximized ? null : imageSrc);
  };

  /// Adds a mousemove event listener to track the cursor position when an image is maximized
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

  /// Renders a carousel with custom thumbnails
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

  /// Renders either a single image or a carousel based on the content provided
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

  /// Renders either the images or a loader while content is being fetched
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

  /// Returns early if content is not available
  if (!content) {
    return (
      <div className={styles.mainContent}>Content not available</div>
    );
  }

  /// Maps the activeTab to the corresponding content section
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
    /// Applies the laser cursor and maximized image when the image is clicked
    <div className={`${styles.mainContent} ${maximizedImage ? styles.laserCursorEnabled : ""}`}>
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
