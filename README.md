import React, { useState, useEffect } from "react";
import { Carousel } from "react-responsive-carousel";
import "react-responsive-carousel/lib/styles/carousel.min.css";
import styles from "./MainContent.module.css";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faTimes } from "@fortawesome/free-solid-svg-icons";
import Video from "./Video";
import { ClipLoader } from "react-spinners";

const MainContent = ({ activeTab, content }) => {
  const [maximizedImage, setMaximizedImage] = useState(null);
  const [currentSlide, setCurrentSlide] = useState(0);
  const [isLoading, setIsLoading] = useState(true);
  const [imagesAvailable, setImagesAvailable] = useState(false);

  useEffect(() => {
    const checkImages = async () => {
      if (content && Array.isArray(content) && content.length > 0) {
        const imagePromises = content.map(
          (src) =>
            new Promise((resolve) => {
              const img = new Image();
              img.src = src;
              img.onload = () => resolve(true);
              img.onerror = () => resolve(false);
            })
        );

        const results = await Promise.all(imagePromises);
        const hasImages = results.some((result) => result === true);

        setImagesAvailable(hasImages);
      }
      setIsLoading(false);
    };

    checkImages();
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
            <img
              src={image}
              alt={`Thumbnail ${index + 1}`}
              className={styles.customThumb}
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

  const renderContent = (images) => {
    if (isLoading) {
      return (
        <div className={styles.loaderContainer}>
          <ClipLoader color="#5f1ec1" loading={isLoading} size={50} />
        </div>
      );
    }

    if (!imagesAvailable) {
      return <div>No images available</div>;
    }

    return images.length > 1 ? renderCarousel(images) : (
      <img
        src={images[0]}
        alt="Single Image"
        className={maximizedImage === images[0] ? styles.maximized : ""}
        onClick={() => toggleMaximize(images[0])}
      />
    );
  };

  const renderImageOrCarousel = (images) => {
    return renderContent(images);
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
        <div
          className={styles.overlay}
          onClick={() => setMaximizedImage(null)}
        >
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
