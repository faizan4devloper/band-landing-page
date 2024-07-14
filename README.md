import React, { useRef, useCallback, useState, memo, Suspense } from "react";
import { BrowserRouter as Router, Routes, Route, Link } from "react-router-dom";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faArrowRight, faArrowLeft, faChevronDown } from "@fortawesome/free-solid-svg-icons";
import styles from "./App.module.css";
import { cardsData } from "./data";
import { FixedSizeList } from 'react-window'; // Import react-window for virtualization

const Header = React.lazy(() => import("./components/Header/Header"));
const MyCarousel = React.lazy(() => import("./components/Carousel/MyCarousel"));
const Cards = React.lazy(() => import("./components/Cards/Cards"));
const SideBarPage = React.lazy(() => import("./components/Sidebar/SideBarPage"));
const AllCardsPage = React.lazy(() => import("./components/Cards/AllCardsPage"));

const App = () => {
  const [bigIndex, setBigIndex] = useState(null);
  const cardsContainerRef = useRef(null);

  const toggleSize = useCallback(
    (index) => {
      setBigIndex(index === bigIndex ? null : index);
    },
    [bigIndex]
  );

  const handleClickLeft = useCallback(() => {
    setBigIndex((prevIndex) => (prevIndex === null || prevIndex === 0 ? cardsData.length - 1 : prevIndex - 1));
  }, []);

  const handleClickRight = useCallback(() => {
    setBigIndex((prevIndex) => (prevIndex === null || prevIndex === cardsData.length - 1 ? 0 : prevIndex + 1));
  }, []);

  const handleScrollDown = useCallback(() => {
    if (cardsContainerRef.current) {
      cardsContainerRef.current.scrollIntoView({ behavior: "smooth" });
    }
  }, []);

  return (
    <Router>
      <div className={styles.app}>
        <Suspense fallback={<div>Loading...</div>}>
          <Header />
        </Suspense>
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
          <Route
            path="/dashboard"
            element={
              <Suspense fallback={<div>Loading...</div>}>
                <SideBarPage />
              </Suspense>
            }
          />
          <Route
            path="/all-cards"
            element={
              <Suspense fallback={<div>Loading...</div>}>
                <AllCardsPage cardsData={cardsData} cardsContainerRef={cardsContainerRef} />
              </Suspense>
            }
          />
        </Routes>
        <div className={styles.scrollDownButton} onClick={handleScrollDown} title="Scroll Down">
          <FontAwesomeIcon icon={faChevronDown} />
        </div>
      </div>
    </Router>
  );
};

const Home = memo(({ cardsData, handleClickLeft, handleClickRight, bigIndex, toggleSize, cardsContainerRef }) => {
  const renderCard = ({ index, style }) => (
    <div style={style}>
      <Suspense fallback={<div>Loading...</div>}>
        <Cards
          imageUrl={cardsData[index].imageUrl}
          title={cardsData[index].title}
          description={cardsData[index].description}
          isBig={index === bigIndex}
          toggleSize={() => toggleSize(index)}
        />
      </Suspense>
    </div>
  );

  return (
    <>
      <Suspense fallback={<div>Loading...</div>}>
        <MyCarousel />
      </Suspense>
      <div className={styles.cardsContainer} ref={cardsContainerRef}>
        <div className={styles.viewAllContainer}>
          <Link to="/all-cards" className={styles.viewAllButton}>
            View All Solutions <FontAwesomeIcon icon={faArrowRight} className={styles.icon} />
          </Link>
        </div>
        <span className={`${styles.arrow} ${styles.leftArrow}`} onClick={handleClickLeft}>
          <FontAwesomeIcon icon={faArrowLeft} title="Previous" />
        </span>
        <FixedSizeList
          height={400} // Set height of the list
          width="100%" // Set width of the list
          itemSize={150} // Set height of each item
          itemCount={cardsData.length} // Number of items in the list
        >
          {renderCard}
        </FixedSizeList>
        <span className={`${styles.arrow} ${styles.rightArrow}`} onClick={handleClickRight}>
          <FontAwesomeIcon icon={faArrowRight} title="Next" />
        </span>
      </div>
    </>
  );
});

export default App;
