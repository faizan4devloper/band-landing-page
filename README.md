import React, { useState, useEffect, useRef } from "react";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faArrowRight, faArrowLeft } from "@fortawesome/free-solid-svg-icons";
import MyCarousel from "./components/Carousel/MyCarousel";
import Cards from "./components/Cards/Cards";
import styles from "./App.module.css";
import { Link } from "react-router-dom";

const Home = ({
  cardsData,
  handleClickLeft,
  handleClickRight,
  currentIndex,
  bigIndex,
  toggleSize,
  cardsContainerRef,
  handleMouseEnter,
}) => {
  const [videoSize, setVideoSize] = useState("small"); // State to manage video size
  const videoRef = useRef(null);

  const handleScroll = () => {
    if (videoRef.current) {
      const videoPosition = videoRef.current.getBoundingClientRect().top;
      const triggerPoint = window.innerHeight / 2; // Adjust trigger point as needed

      if (videoPosition <= triggerPoint) {
        setVideoSize("big");
      } else {
        setVideoSize("small");
      }
    }
  };

  useEffect(() => {
    window.addEventListener("scroll", handleScroll);
    return () => window.removeEventListener("scroll", handleScroll);
  }, []);

  const visibleCards = cardsData.slice(currentIndex, currentIndex + 5);

  return (
    <>
      <MyCarousel />
      <div className={`${styles.videoContainer} ${styles[videoSize]}`} ref={videoRef}>
        <video
          className={styles.video}
          src="path/to/your/video.mp4"
          controls
          muted
          autoPlay
          loop
        />
      </div>
      <div
        className={styles.cardsContainer}
        ref={cardsContainerRef}
        onMouseEnter={handleMouseEnter}
      >
        <div className={styles.viewAllContainer}>
          <Link to="/all-cards" className={styles.viewAllButton}>
            View All Solutions <FontAwesomeIcon icon={faArrowRight} className={styles.icon} />
          </Link>
        </div>
        <span className={`${styles.arrow} ${styles.leftArrow}`} onClick={handleClickLeft}>
          <FontAwesomeIcon icon={faArrowLeft} title="Previous" />
        </span>
        {visibleCards.map((card, index) => {
          const actualIndex = currentIndex + index;
          return (
            <Cards
              key={index}
              imageUrl={card.imageUrl}
              title={card.title}
              description={card.description}
              isBig={actualIndex === bigIndex}
              toggleSize={() => toggleSize(actualIndex)}
            />
          );
        })}
        <span className={`${styles.arrow} ${styles.rightArrow}`} onClick={handleClickRight}>
          <FontAwesomeIcon icon={faArrowRight} title="Next" />
        </span>
      </div>
    </>
  );

export default Home;




  import React, { useRef, useState, useEffect } from "react";
import { BrowserRouter as Router, Routes, Route, Link, useLocation } from "react-router-dom";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faArrowRight, faArrowLeft, faChevronDown, faChevronUp } from "@fortawesome/free-solid-svg-icons";
import Header from "./components/Header/Header";
import Home from "./Home"; // Import the updated Home component
import SideBarPage from "./components/Sidebar/SideBarPage";
import AllCardsPage from "./components/Cards/AllCardsPage";
import { cardsData as initialCardsData } from "./data";
import { BeatLoader } from "react-spinners"; // Import loaders
import styles from "./App.module.css"; // Ensure this is imported for styling

const MainApp = () => {
  const location = useLocation();
  const [cardsData, setCardsData] = useState(initialCardsData);
  const [currentIndex, setCurrentIndex] = useState(0);
  const [bigIndex, setBigIndex] = useState(null); // Set to null initially
  const [showScrollDown, setShowScrollDown] = useState(true);
  const [showScrollUp, setShowScrollUp] = useState(false);
  const [loading, setLoading] = useState(true); // Added loading state
  const cardsContainerRef = useRef(null);

  const toggleSize = (index) => {
    setBigIndex(index === bigIndex ? null : index); // Handle null state
  };

  const handleClickLeft = () => {
    const newBigIndex = bigIndex === null || bigIndex === 0 ? cardsData.length - 1 : bigIndex - 1;
    setBigIndex(newBigIndex);

    const newCurrentIndex = newBigIndex < currentIndex ? newBigIndex : currentIndex;
    setCurrentIndex(newCurrentIndex);
  };

  const handleClickRight = () => {
    const newBigIndex = bigIndex === null || bigIndex === cardsData.length - 1 ? 0 : bigIndex + 1;
    setBigIndex(newBigIndex);

    const newCurrentIndex = newBigIndex > currentIndex + 4 ? newBigIndex - 4 : currentIndex;
    setCurrentIndex(newCurrentIndex);
  };

  const handleScrollDown = () => {
    if (cardsContainerRef.current) {
      cardsContainerRef.current.scrollIntoView({ behavior: "smooth" });
      setShowScrollDown(false);
      setShowScrollUp(true);
    }
  };

  const handleScrollUp = () => {
    window.scrollTo({ top: 0, behavior: "smooth" });
    setShowScrollDown(true);
    setShowScrollUp(false);
  };

  const handleMouseEnter = () => {
    if (cardsContainerRef.current) {
      cardsContainerRef.current.scrollIntoView({ behavior: "smooth", block: "start" });
    }
  };

  useEffect(() => {
    const handleScroll = () => {
      if (window.scrollY > 50) {
        setShowScrollUp(true);
        setShowScrollDown(false);
      } else {
        setShowScrollUp(false);
        setShowScrollDown(true);
      }
    };

    const debounceScroll = debounce(handleScroll, 100);
    window.addEventListener("scroll", debounceScroll);

    return () => window.removeEventListener("scroll", debounceScroll);
  }, []);

  useEffect(() => {
    // Simulate a delay to show the loader
    const timer = setTimeout(() => {
      setLoading(false);
    }, 2000); // 2 seconds delay

    return () => clearTimeout(timer);
  }, []);

  const debounce = (func, wait) => {
    let timeout;
    return function executedFunction(...args) {
      const later = () => {
        clearTimeout(timeout);
        func(...args);
      };
      clearTimeout(timeout);
      timeout = setTimeout(later, wait);
    };
  };

  return (
    <div className={styles.app}>
      {loading ? ( // Show loader while loading is true
        <div className={styles.loader}>
          <BeatLoader color="#5931d5" loading={loading} size={15} margin={2} />
        </div>
      ) : (
        <>
          <Header />
          <Routes>
            <Route
              path="/"
              element={
                <Home
                  cardsData={cardsData}
                  handleClickLeft={handleClickLeft}
                  handleClickRight={handleClickRight}
                  currentIndex={currentIndex}
                  bigIndex={bigIndex}
                  toggleSize={toggleSize}
                  cardsContainerRef={cardsContainerRef}
                  handleMouseEnter={handleMouseEnter}
                />
              }
            />
            <Route path="/dashboard" element={<SideBarPage />} />
            <Route
              path="/all-cards"
              element={<AllCardsPage cardsData={cardsData} cardsContainerRef={cardsContainerRef} />}
            />
          </Routes>
          {showScrollDown && location.pathname !== "/all-cards" && location.pathname !== "/dashboard" && (
            <div className={styles.scrollDownButton} onClick={handleScrollDown} title="Scroll Down">
              <FontAwesomeIcon icon={faChevronDown} />
            </div>
          )}
          {showScrollUp && (
            <div className={styles.scrollUpButton} onClick={handleScrollUp} title="Scroll Up">
              <FontAwesomeIcon icon={faChevronUp} />
            </div>
          )}
        </>
      )}
    </div>
  );
};

const App = () => (
  <Router>
    <MainApp />
  </Router>
);

export default App;
};





/* Video Resizing Effect */
.videoContainer {
  width: 100%;
  display: flex;
  justify-content: center;
  margin: 20px 0; /* Space around the video */
  transition: transform 0.6s ease, width 0.6s ease, height 0.6s ease;
}

.videoContainer.small {
  transform: scale(0.8); /* Small state */
  width: 60%; /* Adjust width for small state */
  height: auto; /* Maintain aspect ratio */
  }


.videoContainer.big {
  transform: scale(1); /* Big state (normal size) */
  width: 100%; /* Full width in big state */
  height: auto; /* Maintain aspect ratio */
}

.video {
  width: 100%;
  height: auto;
  border-radius: 10px; /* Optional: add some border radius for smoother look */
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2); /* Optional: shadow for depth */
}

/* Scroll Buttons */
.scrollDownButton,
.scrollUpButton {
  position: fixed;
  bottom: 30px;
  right: 30px;
  background-color: #5931d5;
  color: white;
  padding: 10px;
  border-radius: 50%;
  cursor: pointer;
  z-index: 1000;
  transition: background-color 0.3s ease;
}

.scrollDownButton:hover,
.scrollUpButton:hover {
  background-color: #4b29b1; /* Slightly darker on hover */
}

/* Loader Styles */
.loader {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background-color: #f4f4f4;
}

.viewAllContainer {
  text-align: center;
  margin-bottom: 20px;
}

.viewAllButton {
  display: inline-flex;
  align-items: center;
  background-color: #5931d5;
  color: #fff;
  padding: 10px 20px;
  border-radius: 5px;
  text-decoration: none;
  transition: background-color 0.3s ease;
}

.viewAllButton:hover {
  background-color: #4b29b1;
}

.viewAllButton .icon {
  margin-left: 10px;
}

/* Other styles for the application */
.app {
  font-family: "Arial", sans-serif;
}

.arrow {
  cursor: pointer;
  font-size: 1.5rem;
  color: #5931d5;
}

.leftArrow {
  position: absolute;
  left: 10px;
  top: 50%;
  transform: translateY(-50%);
}

.rightArrow {
  position: absolute;
  right: 10px;
  top: 50%;
  transform: translateY(-50%);
}

.cardsContainer {
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
  overflow: hidden;
  padding: 20px;
}
