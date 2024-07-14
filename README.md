import React, { useRef, useCallback, useState, memo, Suspense } from "react";
import { BrowserRouter as Router, Routes, Route, Link } from "react-router-dom";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faArrowRight, faArrowLeft, faChevronDown } from "@fortawesome/free-solid-svg-icons";
import styles from "./App.module.css";
import { cardsData } from "./data";
import { FixedSizeList } from 'react-window';

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
        <div style={{ display: 'flex', flexWrap: 'wrap', gap: '20px', justifyContent: 'flex-start', width: '100%' }}>
          <FixedSizeList
            height={400} // Set height of the list
            width="100%" // Set width of the list
            itemSize={150} // Set height of each item
            itemCount={cardsData.length} // Number of items in the list
          >
            {renderCard}
          </FixedSizeList>
        </div>
        <span className={`${styles.arrow} ${styles.rightArrow}`} onClick={handleClickRight}>
          <FontAwesomeIcon icon={faArrowRight} title="Next" />
        </span>
      </div>
    </>
  );
});

export default App;





@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600;700&display=swap");

/* Hide the scrollbar */
::-webkit-scrollbar {
  width: 0;
  height: 0;
}

html,
body {
  overflow-y: scroll;
  font-family: "Poppins", sans-serif;
  scroll-behavior: smooth; /* Smooth scrolling */
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
  justify-content: flex-start; /* Align items in a row */
  flex-wrap: wrap; /* Allow items to wrap onto multiple lines */
  will-change: transform; /* Optimize for scrolling */
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
  color: rgba(15, 95, 220, 1);
  transition: transform 0.5s ease, background 0.5s ease;
}

.arrow:hover {
  background-color: rgba(15, 95, 220, 1);
  color: white;
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

.viewAllContainer {
  width: 100%;
  display: flex;
  justify-content: flex-end;
  align-items: center;
  margin-right: 112px;
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
  transform: translateY(-5px);
  color: #5f1ec1;
  background-color: rgba(13, 85, 198, 0.1);
  box-shadow: 0px 8px 15px rgba(0, 0, 0, 0.2);
}

.icon {
  margin-left: 8px;
  transition: transform 0.3s ease;
}

.viewAllButton:hover .icon {
  transform: translateX(5px);
}

.scrollDownButton {
  position: fixed;
  left: 20px;
  bottom: 20px;
  background: linear-gradient(90deg, #6f36cd 0%, #1f77f6 100%);
  color: white;
  border: none;
  border-radius: 4px;
  width: 21px;
  height: 22px;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 12px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.scrollDownButton:hover {
  background-color: rgba(13, 85, 198, 1);
}
