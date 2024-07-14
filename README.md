import React, { useRef, useCallback, useState, Suspense } from "react";
import { BrowserRouter as Router, Routes, Route, Link } from "react-router-dom";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faArrowRight, faArrowLeft, faChevronDown } from "@fortawesome/free-solid-svg-icons";
import styles from "./App.module.css";
import { cardsData } from "./data";

const Header = React.lazy(() => import("./components/Header/Header"));
const MyCarousel = React.lazy(() => import("./components/Carousel/MyCarousel"));
const Home = React.lazy(() => import("./Home")); // Import the updated Home component
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

  // Render card function to be passed down to Home component
  const renderCard = useCallback(
    ({ index, style }) => (
      <Suspense fallback={<div>Loading...</div>}>
        <Cards
          key={index}
          imageUrl={cardsData[index].imageUrl}
          title={cardsData[index].title}
          description={cardsData[index].description}
          isBig={index === bigIndex}
          toggleSize={() => toggleSize(index)}
          style={style} // Pass style to adjust positioning
        />
      </Suspense>
    ),
    [bigIndex, toggleSize]
  );

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
                renderCard={renderCard} // Pass the renderCard function
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

export default App;
