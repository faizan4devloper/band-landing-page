import React, { useRef, useCallback, memo } from "react";
import { BrowserRouter as Router, Routes, Route, Link } from "react-router-dom";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faArrowRight, faArrowLeft, faChevronDown } from "@fortawesome/free-solid-svg-icons";
import Header from "./components/Header/Header";
import MyCarousel from "./components/Carousel/MyCarousel";
import Cards from "./components/Cards/Cards";
import styles from "./App.module.css";
import SideBarPage from "./components/Sidebar/SideBarPage";
import AllCardsPage from "./components/Cards/AllCardsPage"; 
import { cardsData } from "./data";

const App = () => {
  const [bigIndex, setBigIndex] = React.useState(null);
  const cardsContainerRef = useRef(null);

  const toggleSize = useCallback((index) => {
    setBigIndex(index === bigIndex ? null : index);
  }, [bigIndex]);

  const handleClickLeft = useCallback(() => {
    setBigIndex(prevIndex => (prevIndex === null || prevIndex === 0 ? cardsData.length - 1 : prevIndex - 1));
  }, []);

  const handleClickRight = useCallback(() => {
    setBigIndex(prevIndex => (prevIndex === null || prevIndex === cardsData.length - 1 ? 0 : prevIndex + 1));
  }, []);

  const handleScrollDown = useCallback(() => {
    if (cardsContainerRef.current) {
      cardsContainerRef.current.scrollIntoView({ behavior: "smooth" });
    }
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
                bigIndex={bigIndex}
                toggleSize={toggleSize}
                cardsContainerRef={cardsContainerRef}
              />
            }
          />
          <Route path="/dashboard" element={<SideBarPage />} />
          <Route path="/all-cards" element={<AllCardsPage cardsData={cardsData} cardsContainerRef={cardsContainerRef} />} />
        </Routes>
        <div className={styles.scrollDownButton} onClick={handleScrollDown} title="Scroll Down">
          <FontAwesomeIcon icon={faChevronDown} />
        </div>
      </div>
    </Router>
  );
};

const Home = memo(({ cardsData, handleClickLeft, handleClickRight, bigIndex, toggleSize, cardsContainerRef }) => {
  return (
    <>
      <MyCarousel />
      <div className={styles.cardsContainer} ref={cardsContainerRef}>
        <div className={styles.viewAllContainer}>
          <Link to="/all-cards" className={styles.viewAllButton}>
            View All Solutions <FontAwesomeIcon icon={faArrowRight} className={styles.icon} />
          </Link>
        </div>
        <span className={`${styles.arrow} ${styles.leftArrow}`} onClick={handleClickLeft}>
          <FontAwesomeIcon icon={faArrowLeft} title="Previous" />
        </span>
        {cardsData.slice(0, 5).map((card, index) => (
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
});

export default App;
