import React, { useRef } from "react";
import { BrowserRouter as Router, Routes, Route, Link, useLocation } from "react-router-dom";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faArrowRight, faArrowLeft, faChevronDown } from "@fortawesome/free-solid-svg-icons";
import Header from "./components/Header/Header";
import MyCarousel from "./components/Carousel/MyCarousel";
import Cards from "./components/Cards/Cards";
import styles from "./App.module.css";
import SideBarPage from "./components/Sidebar/SideBarPage";
import AllCardsPage from "./components/Cards/AllCardsPage"; // Import the new component
import { cardsData } from "./data"; // Import card data from a separate file

const App = () => {
  return (
    <Router> {/* Ensure Router is wrapping your entire application */}
      <Main />
    </Router>
  );
};

const Main = () => {
  const [bigIndex, setBigIndex] = React.useState(null);
  const cardsContainerRef = useRef(null);
  const location = useLocation(); // Get the current route location

  const toggleSize = (index) => {
    setBigIndex(index === bigIndex ? null : index);
  };

  const handleClickLeft = () => {
    if (bigIndex === null || bigIndex === 0) {
      setBigIndex(cardsData.length - 1);
    } else {
      setBigIndex(bigIndex - 1);
    }
  };

  const handleClickRight = () => {
    if (bigIndex === null || bigIndex === cardsData.length - 1) {
      setBigIndex(0);
    } else {
      setBigIndex(bigIndex + 1);
    }
  };

  const handleScrollDown = () => {
    window.scrollTo({
      top: document.documentElement.scrollHeight,
      behavior: "smooth",
    });
  };

  return (
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
        <Route
          path="/all-cards"
          element={<AllCardsPage cardsData={cardsData} cardsContainerRef={cardsContainerRef} />}
        />
      </Routes>
      {/* Conditionally render the scroll down button */}
      {location.pathname !== "/all-cards" && (
        <div className={styles.scrollDownButton} onClick={handleScrollDown} title="Scroll Down">
          <FontAwesomeIcon icon={faChevronDown} />
        </div>
      )}
    </div>
  );
};

const Home = ({
  cardsData,
  handleClickLeft,
  handleClickRight,
  bigIndex,
  toggleSize,
  cardsContainerRef,
}) => {
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
};

export default App;
