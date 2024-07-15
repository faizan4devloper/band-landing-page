import React, { useRef, useState, useEffect } from "react";
import { BrowserRouter as Router, Routes, Route, Link } from "react-router-dom";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faArrowRight, faArrowLeft, faChevronDown, faChevronUp } from "@fortawesome/free-solid-svg-icons";
import Header from "./components/Header/Header";
import MyCarousel from "./components/Carousel/MyCarousel";
import Cards from "./components/Cards/Cards";
import styles from "./App.module.css";
import SideBarPage from "./components/Sidebar/SideBarPage";
import AllCardsPage from "./components/Cards/AllCardsPage";
import { cardsData } from "./data";

const App = () => {
  const [currentIndex, setCurrentIndex] = useState(0);
  const [bigIndex, setBigIndex] = useState(null);
  const [showScrollDown, setShowScrollDown] = useState(true);
  const [showScrollUp, setShowScrollUp] = useState(false);
  const cardsContainerRef = useRef(null);

  const toggleSize = (index) => {
    setBigIndex(index === bigIndex ? null : index);
  };

  const handleClickLeft = () => {
    setCurrentIndex(currentIndex === 0 ? cardsData.length - 5 : currentIndex - 1);
  };

  const handleClickRight = () => {
    setCurrentIndex(currentIndex === cardsData.length - 5 ? 0 : currentIndex + 1);
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

    window.addEventListener("scroll", handleScroll);
    return () => window.removeEventListener("scroll", handleScroll);
  }, []);

  return (
    <Router>
      <div className={styles.app}>
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
        {showScrollDown && (
          <div className={styles.scrollDownButton} onClick={handleScrollDown} title="Scroll Down">
            <FontAwesomeIcon icon={faChevronDown} />
          </div>
        )}
        {showScrollUp && (
          <div className={styles.scrollUpButton} onClick={handleScrollUp} title="Scroll Up">
            <FontAwesomeIcon icon={faChevronUp} />
          </div>
        )}
      </div>
    </Router>
  );
};

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
  const visibleCards = cardsData.slice(currentIndex, currentIndex + 5);
  return (
    <>
      <MyCarousel />
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
        {visibleCards.map((card, index) => (
          <Cards
            key={index}
            imageUrl={card.imageUrl}
            title={card.title}
            description={card.description}
            isBig={index === bigIndex}
            toggleSize={() => toggleSize(index)}
          />
        ))}
        <span className={`${styles.arrow} ${styles.rightArrow}`} onClick={handleClickRight}>
          <FontAwesomeIcon icon={faArrowRight} title="Next" />
        </span>
      </div>
    </>
  );
};

export default App;






@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600;700&display=swap");

/* Hide the scrollbar */
::-webkit-scrollbar {
  width: 0; /* Remove scrollbar width */
  height: 0; /* Remove scrollbar height */
}

html,
body {
  overflow-y: scroll; /* Always show vertical scrollbar */
  font-family: "Poppins", sans-serif;
}

.app {
  width: 1100px;
  height: 600px;
  margin: 0 auto;
}

.cardsContainer {
  gap: 20px;
  border-radius: 12px;
  display: flex;
  justify-content: center;
}

.arrow {
  cursor: pointer;
  position: relative;
  top: 50%;
  transform: translateY(-50%);
  font-size: 18px;
  width: 17px;
    height: 22px;
    padding: 3px 8px 5px 6px;
  border-radius: 50px;
  border: 2px solid rgba(15, 95, 220, 1);
  color: rgba(15, 95, 220, 1); /* Adjust color as needed */
  transition: transform 0.5s ease, background 0.5s ease; /* Add transition */
}

.arrow:hover {
  background-color: rgba(15, 95, 220, 1); /* Change background color on hover */
  color: white; /* Change text color on hover */
}

.leftArrow {
  left: -25px;
  top: 90px;
}

.rightArrow {
  right: -25px;
  top: 90px;
}

@media screen and (max-width: 1100px) {
  .app {
    padding: 0 10px;
  }
}

@media screen and (max-width: 768px) {
  .app {
    max-width: 100%;
  }
}


.arrow img::after {
  content: attr(title);
  position: absolute;
  top: 100%;
  left: 50%;
  transform: translateX(-50%);
  background-color: #333;
  color: #fff;
  padding: 4px;
  border-radius: 5px;
  font-size: 8px;
  white-space: nowrap;
  opacity: 0;
  visibility: hidden;
  /*transition: opacity 0.1s, visibility 0.1s;*/
}

.logo img:hover::after {
  opacity: 1;
  visibility: visible;
}

.viewAllContainer {
  width: 100%;
  display: flex;
  justify-content: right;
  align-items: center;
  /*margin-bottom: 20px;*/
  margin-right: 112px;
}

.solutionHead{
    font-weight: 600;
    font-size: 14px;
    color: #808080;
      margin-left: 118px;

}



.viewAllButton {
  font-weight: 600;
  font-size: 14px;
  color: #808080;
  cursor: pointer;
  transition: background-color 0.3s ease, transform 0.3s ease, color 0.3s ease;
  text-decoration: none;
  display: inline-flex; 
  align-items: center; 
  padding: 6px 7px; 
  border-radius: 5px; 
  box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.1); 
}

.viewAllButton:hover {
  transform: translateY(-5px); /* Adds a smooth lift on hover */
  color: #5f1ec1;
  background-color: rgba(13, 85, 198, 0.1); /* Adds a subtle background color change on hover */
  box-shadow: 0px 8px 15px rgba(0, 0, 0, 0.2); /* Enhances the shadow effect on hover */
}

.icon {
  margin-left: 8px; /* Adds space between the text and the icon */
  transition: transform 0.3s ease; /* Smooth transition for the icon */
}

.viewAllButton:hover .icon {
  transform: translateX(5px); /* Moves the icon slightly to the right on hover */
}


.cardsContainer {
  gap: 20px;
  border-radius: 12px;
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
}

.scrollDownButton {
  position: fixed;
  left: 20px; /* Adjust as per your layout */
  bottom: 20px; /* Adjust as per your layout */
  background: linear-gradient(90deg, #6f36cd 0%, #1f77f6 100%);
  color: white;
  border: none;
  cursor: pointer;
  border-radius: 4px;
  width: 21px;
  height: 22px;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 12px;
  cursor: pointer;
  transition: background-color 0.3s ease;
    transition: .4s ease

}

.scrollDownButton:hover {
  background-color: rgba(13, 85, 198, 1); /* Darker shade on hover */
}
.scrollUpButton {
  position: fixed;
  left: 20px; /* Adjust as per your layout */
  bottom: 20px; /* Adjust as per your layout */
  background: linear-gradient(90deg, #6f36cd 0%, #1f77f6 100%);
  color: white;
  border: none;
  cursor: pointer;
  border-radius: 4px;
  width: 21px;
  height: 22px;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 12px;
  cursor: pointer;
  transition: background-color 0.3s ease;
  transition: .4s ease
}

.scrollUpButton:hover {
  background-color: rgba(13, 85, 198, 1); /* Darker shade on hover */
}
