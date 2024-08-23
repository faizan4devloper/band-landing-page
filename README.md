import React, { useState, useEffect } from "react";
import { Carousel } from "react-responsive-carousel";
import "react-responsive-carousel/lib/styles/carousel.min.css"; // Import carousel styles
import styles from "./MainContent.module.css";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faTimes } from "@fortawesome/free-solid-svg-icons";
import Video from "./Video";
import Loader from "./Loader"; // Import a Loader component

const MainContent = ({ activeTab, content }) => {
  const [maximizedImage, setMaximizedImage] = useState(null);
  const [currentSlide, setCurrentSlide] = useState(0);
  const [isLoading, setIsLoading] = useState(true); // Add loading state

  useEffect(() => {
    if (content) {
      setIsLoading(false); // Simulate image/content loading
    }
  }, [content]);

  const toggleMaximize = (imageSrc) => {
    setMaximizedImage(maximizedImage === imageSrc ? null : imageSrc);
  };

  const renderCarousel = (images) => (
    <div className={styles.carouselContainer}>
      <div className={styles.customThumbs}>
        {images.map((image, index) => (
          <div
            key={index}
            className={`${styles.customThumbContainer} ${
              currentSlide === index ? styles.selected : ""
            }`}
            onClick={() => setCurrentSlide(index)}
          >
            <img src={image} alt={`Thumbnail ${index + 1}`} className={styles.customThumb} />
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
          <div key={index}>
            <img
              src={image}
              alt={`Slide ${index + 1}`}
              className={maximizedImage === image ? styles.maximized : ""}
              onClick={() => toggleMaximize(image)}
            />
          </div>
        ))}
      </Carousel>
    </div>
  );

  const renderContent = (content) => {
    if (!content || content.length === 0) {
      return <Loader />; // Display loader instead of "No content available"
    }

    if (typeof content === "string") {
      return <div>{content}</div>;
    }

    return content.length > 1 ? (
      renderCarousel(content)
    ) : (
      <img
        src={content[0]}
        alt="Single Image"
        className={maximizedImage === content[0] ? styles.maximized : ""}
        onClick={() => toggleMaximize(content[0])}
      />
    );
  };

  const renderImageOrCarousel = (images) => {
    if (isLoading) {
      return <Loader />; // Display loader while loading
    }

    return renderContent(images); // Render content if available
  };

  if (!content) {
    return <div className={styles.mainContent}>Content not available</div>;
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
    <div className={styles.mainContent}>
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
            className={styles.maximized}
          />
        </div>
      )}
    </div>
  );
};

export default MainContent;
