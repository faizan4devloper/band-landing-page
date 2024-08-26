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
    setMaximizedImage(maximizedImage === imageSrc ? null : imageSrc);
  };

  const handleMouseMove = (e) => {
    setLaserPos({ x: e.clientX, y: e.clientY });
  };

  useEffect(() => {
    document.addEventListener("mousemove", handleMouseMove);
    return () => {
      document.removeEventListener("mousemove", handleMouseMove);
    };
  }, []);

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
  z-index: 1101;
}

.maximizedImage {
  width: 77%;
  height: 100%;
  object-fit: contain;
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




import React, { useState, useEffect, useRef } from "react";
import Modal from "react-modal";
import { useNavigate } from "react-router-dom";
import styles from "./Header.module.css";
import logoImage from "./HCLTechLogo.svg";
import RequestDemoForm from "./RequestDemoForm";
import laserStyles from "./LaserCursor.module.css";

const Header = () => {
  const [modalIsOpen, setModalIsOpen] = useState(false);
  const [isVisible, setIsVisible] = useState(true);
  const [isLaserEnabled, setIsLaserEnabled] = useState(false);
  const [cursorPosition, setCursorPosition] = useState({ x: 0, y: 0 });

  const [lastScrollY, setLastScrollY] = useState(0);
  const navigate = useNavigate();
  const laserCursorRef = useRef(null);

  const handleImageClick = () => {
    navigate("/");
  };

  const openModal = () => {
    setModalIsOpen(true);
  };

  const closeModal = () => {
    setModalIsOpen(false);
  };

  const controlHeaderVisibility = () => {
    if (window.scrollY > lastScrollY) {
      setIsVisible(false);
    } else {
      setIsVisible(true);
    }
    setLastScrollY(window.scrollY);
  };

  const toggleLaserCursor = () => {
    setIsLaserEnabled((prev) => !prev);
  };

  useEffect(() => {
    window.addEventListener("scroll", controlHeaderVisibility);
    return () => {
      window.removeEventListener("scroll", controlHeaderVisibility);
    };
  }, [lastScrollY]);

  useEffect(() => {
    const handleMouseMove = (e) => {
      setCursorPosition({ x: e.clientX, y: e.clientY });
    };

    if (isLaserEnabled) {
      document.body.classList.add("laser-enabled");
      document.body.style.cursor = 'none'; // Force hide default cursor
      window.addEventListener("mousemove", handleMouseMove);
    } else {
      document.body.classList.remove("laser-enabled");
      document.body.style.cursor = ''; // Reset cursor style
      window.removeEventListener("mousemove", handleMouseMove);
    }

    return () => {
      window.removeEventListener("mousemove", handleMouseMove);
    };
  }, [isLaserEnabled]);

  return (
    <div className={`${styles.navbarWrapper} ${isVisible ? styles.show : styles.hide}`}>
      <nav className={styles.header}>
        <div className={styles.logo}>
          <img
            src={logoImage}
            alt=""
            onClick={handleImageClick}
            style={{ cursor: "pointer" }}
            title="Navigate to Home"
          />
        </div>
        <div className={styles.right}>
          <button className={styles.button} onClick={openModal}>
            Request For Live Demo
          </button>
          <button className={styles.button} onClick={toggleLaserCursor}>
            {isLaserEnabled ? "Disable Laser Cursor" : "Enable Laser Cursor"}
          </button>
        </div>
      </nav>
      <div className={styles.border}></div>

      <Modal
        isOpen={modalIsOpen}
        onRequestClose={closeModal}
        contentLabel="Request for Live Demo!"
        className={styles.modal}
        overlayClassName={styles.overlay}
      >
        <RequestDemoForm closeModal={closeModal} />
      </Modal>

      {isLaserEnabled && (
        <div
          ref={laserCursorRef}
          className={laserStyles.laserCursor}
          style={{
            left: `${cursorPosition.x}px`,
            top: `${cursorPosition.y}px`,
            transform: `translate(-50%, -50%)`, // Center the cursor on the pointer
          }}
        />
      )}
    </div>
  );
};

export default Header;


.navbarWrapper {
  position: fixed;
  top: 0;
  left: 0;
  min-width: 100%;
  background-color: #fff;
  border-bottom: 0.1px solid rgba(219, 197, 255, 1);
  padding: 8px 0px;
  z-index: 1000;
  transition: transform 0.5s ease-in-out;

  /*margin-bottom: 30px;*/
}
.navbarWrapper.hide {
  transform: translateY(-100%);
}

.navbarWrapper.show {
  transform: translateY(0);
}
.navbarWrapper .header {
  display: flex;
  align-items: center;
  justify-content: space-around;
  margin: 5px -200px;
}
.logo img {
  max-height: 20px;
}

.right {
  display: flex;
  align-items: center;
}

.button {
  display: flex;
  align-items: center;
  background: linear-gradient(90deg, #6f36cd 0%, #1f77f6 100%);
  color: white;
  border: none;
  padding: 8px 8px;
  font-size: 12px;
  border-radius: 5px;
  cursor: pointer;
  transition: .6s ease;
}

.button:hover{
    transform: translate(0, -5px);
    
    

}

.content {
  padding-top: 70px; /* Adjust the value to match the height of your header */
}

.logo img::after {
  content: attr(title);
  position: absolute;
  top: 100%;
  left: 50%;
  transform: translateX(-50%);
  background-color: #333;
  color: #fff;
  padding: 5px;
  border-radius: 4px;
  font-size: 12px;
  white-space: nowrap;
  opacity: 0;
  visibility: hidden;
  transition: opacity 0.3s, visibility 0.1s;
}

.logo img:hover::after {
  opacity: 1;
  visibility: visible;
}

@keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}

.modal {
  position: absolute;
  top: 50%;
  left: 50%;
  right: auto;
  bottom: auto;
  transform: translate(-50%, -50%);
  background: #fff;
  border-radius: 8px;
  padding: 20px;
  margin-top: 30px;
  max-width: 900px;
  width: 90%;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  animation: fadeIn 0.3s ease-out;
}

.overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.9);
  animation: fadeIn 0.3s ease-out;
}


