/* Laser Cursor Styling */
.laserCursorEnabled .laserCursor {
  display: block;
}

.laserCursorEnabled * {
  cursor: none !important; /* Hide the default cursor */
}

.laserCursor {
  position: fixed;
  width: 15px; /* Adjust size to resemble a laser pointer */
  height: 15px; /* Adjust size to resemble a laser pointer */
  background-color: #ff0000; /* Red color for the laser cursor */
  border-radius: 50%; /* Make it a circle */
  pointer-events: none;
  z-index: 1500;
  
  /* Shadow and glow effect */
  box-shadow: 0 0 10px rgba(255, 0, 0, 0.8), 0 0 20px rgba(255, 0, 0, 0.6);
  animation: pulseLaser 1.5s infinite;
}

/* Optional laser tail effect */
.laserCursor::after {
  content: '';
  position: absolute;
  width: 30px; /* Length of the tail */
  height: 2px; /* Thickness of the tail */
  background-color: rgba(255, 0, 0, 0.8); /* Tail color with some transparency */
  top: 50%; /* Center vertically */
  left: -30px; /* Position the tail to the left of the cursor */
  transform: translateY(-50%);
  box-shadow: 0 0 5px rgba(255, 0, 0, 0.5);
}

/* Pulse animation to mimic the laser pointer effect */
@keyframes pulseLaser {
  0% {
    transform: scale(1);
    box-shadow: 0 0 10px rgba(255, 0, 0, 0.8), 0 0 20px rgba(255, 0, 0, 0.6);
  }
  50% {
    transform: scale(1.2);
    box-shadow: 0 0 15px rgba(255, 0, 0, 1), 0 0 30px rgba(255, 0, 0, 0.8);
  }
  100% {
    transform: scale(1);
    box-shadow: 0 0 10px rgba(255, 0, 0, 0.8), 0 0 20px rgba(255, 0, 0, 0.6);
  }
}










// JavaScript to handle laser cursor movement
document.addEventListener('mousemove', function (e) {
  const laserCursor = document.querySelector('.laserCursor');
  if (laserCursor) {
    laserCursor.style.left = `${e.pageX}px`;
    laserCursor.style.top = `${e.pageY}px`;
  }
});

// Function to handle image maximization
function maximizeImage(image) {
  const overlay = document.createElement('div');
  overlay.classList.add('overlay');

  const clonedImage = image.cloneNode(true);
  clonedImage.classList.add('maximizedImage');

  const closeIcon = document.createElement('div');
  closeIcon.classList.add('closeIcon');
  closeIcon.textContent = '✕';

  overlay.appendChild(clonedImage);
  overlay.appendChild(closeIcon);
  document.body.appendChild(overlay);

  // Hide default cursor and enable laser cursor
  document.body.classList.add('laserCursorEnabled');

  // Close maximized image on click of close icon or overlay
  closeIcon.addEventListener('click', () => closeMaximizedImage(overlay));
  overlay.addEventListener('click', () => closeMaximizedImage(overlay));
}

function closeMaximizedImage(overlay) {
  overlay.remove();
  // Restore the default cursor
  document.body.classList.remove('laserCursorEnabled');
}

// Add event listeners to all images in .mainContent
document.querySelectorAll('.mainContent img').forEach(img => {
  img.addEventListener('click', function () {
    maximizeImage(this);
  });
});





/* Main content styling */
.mainContent img {
  max-width: 90%;
  height: auto;
  display: block;
  margin: 10px auto;
  cursor: pointer; /* Cursor pointer for images in .mainContent */
  transition: transform 0.3s ease;
  z-index: 1100;
}

/* Hide the cursor for maximized images and overlay */
.maximizedImage, .overlay {
  cursor: none; /* Ensure the default cursor is hidden */
}

/* Laser cursor styling */
.laserCursorEnabled .laserCursor {
  display: block;
}

.laserCursorEnabled * {
  cursor: none !important; /* Force hide the default cursor */
}

.laserCursor {
  position: fixed;
  width: 20px;
  height: 20px;
  background-color: red;
  border-radius: 50%;
  pointer-events: none;
  z-index: 1500;
}

/* Close icon styling */
.closeIcon {
  position: absolute;
  top: 10px;
  right: 100px;
  font-size: 25px;
  color: #ffffff;
  cursor: pointer;
  z-index: 1300;
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
}











/* Main content styling */
.mainContent {
  display: flex;
  flex-direction: column;
  width: 100%;
  padding: 0px 20px;
  background-color: #ffffff;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  height: calc(100vh - 100px);
  overflow-y: auto;
  min-height: 300px;
}

/* Custom Scrollbar Styling */
.mainContent::-webkit-scrollbar {
  width: 12px;
}

.mainContent::-webkit-scrollbar-track {
  background: #f1f1f1;
}

.mainContent::-webkit-scrollbar-thumb {
  background-color: #5f1ec1;
  border-radius: 20px;
  border: 3px solid #f1f1f1;
}

.mainContent::-webkit-scrollbar-thumb:hover {
  background-color: #3d1299;
}

/* Image styling before maximization */
.mainContent img {
  max-width: 90%;
  height: auto;
  display: block;
  margin: 10px auto;
  cursor: pointer; /* Pointer cursor before maximization */
  transition: transform 0.3s ease;
  z-index: 1100;
}

/* Image styling after maximization */
.maximized,
.maximizedImage {
  max-width: 100%;
  max-height: 100%;
  width: 100%;
  height: 100%;
  object-fit: contain;
  margin: auto;
  display: block;
  transition: transform 0.3s ease;
  cursor: none; /* Hide default cursor after maximization */
  z-index: 1200;
}

/* Ensures the cursor is hidden over the maximized image */
.maximizedImage {
  width: 77%;
  height: 100%;
  object-fit: contain;
  cursor: none; /* Hide default cursor after maximization */
  z-index: 1200;
}

/* General cursor hiding for maximized images and overlays */
.maximized,
.maximizedImage,
.overlay {
  cursor: none;
}

/* Close icon styling */
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

/* Section styling */
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

/* Carousel and thumb styling */
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
  width: 100px;
  height: 100px;
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

/* Image loading container */
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

/* Laser cursor enabled */
.laserCursorEnabled {
  display: block;
}

/* Laser cursor styling */
.laserCursor {
  position: fixed;
  width: 20px;
  height: 20px;
  background-color: red;
  border-radius: 50%;
  pointer-events: none;
  z-index: 1500;
}
















.mainContent {
  display: flex;
  flex-direction: column;
  width: 100%;
  padding: 0px 20px;
  background-color: #ffffff;
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
  cursor: none;
  z-index: 1200;
}

.maximizedImage {
  width: 77%;
  cursor: none;
  height: 100%;
  object-fit: contain;
    z-index: 1200;

}
.maximized,
.maximizedImage,
.overlay{
  cursor: none;
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




.laserCursorEnabled{
  display: block;
}


.laserCursor {
  position: fixed;
  width: 20px; /* Adjust size as needed */
  height: 20px; /* Adjust size as needed */
  background-color: red; /* Red color for the laser cursor */
  border-radius: 50%; /* Make it a circle */
  pointer-events: none;
  z-index: 1500;
}



















.laserCursorEnabled {
  cursor: none; /* Hide the default cursor when the laser effect is active */
}

.laserCursor {
  position: fixed;
  width: 20px; /* Adjust size as needed */
  height: 20px; /* Adjust size as needed */
  background-color: red; /* Red color for the laser cursor */
  border-radius: 50%; /* Make it a circle */
  pointer-events: none;
  z-index: 1500;
}

.mainContent {
  display: flex;
  flex-direction: column;
  width: 100%;
  padding: 0px 20px;
  background-color: #ffffff;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  height: calc(100vh - 100px); /* Adjust height as needed */
  overflow-y: auto; /* Enable vertical scroll */
  min-height: 300px;
}

/* Other styles remain unchanged */


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

  const toggleMaximize = (imageSrc) => {
    const isMaximized = maximizedImage === imageSrc;
    setMaximizedImage(isMaximized ? null : imageSrc);
  };

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

  const toggleMaximize = (imageSrc) => {
    const isMaximized = maximizedImage === imageSrc;
    setMaximizedImage(isMaximized ? null : imageSrc);
  };

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


.mainContent {
  display: flex;
  flex-direction: column;
  width: 100%;
  padding: 0px 20px;
  background-color: #ffffff;
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
  z-index: 1500;
}
