import { useState, useEffect } from 'react';

// Custom hook to fetch and store asset URLs from S3
export const useAssetUrls = () => {
  const [assets, setAssets] = useState({
    images: {},
    videos: {},
    solutionFlows: {},
    architectures: {},
    descriptions: {},
    solutionsBenefits: {},
    adoption: {},
  });

  useEffect(() => {
    const fetchAssets = async () => {
      try {
        const response = await fetch('https://your-bucket-name.s3.amazonaws.com/urldata.json');
        if (!response.ok) {
          throw new Error('Failed to fetch asset URLs');
        }
        const data = await response.json();
        setAssets(data);
      } catch (error) {
        console.error('Error fetching asset data:', error);
      }
    };

    fetchAssets();
  }, []);

  return assets;
};




import { useAssetUrls } from './AssetImports';

export const useCardsData = () => {
  const assets = useAssetUrls();
  
  const mapAssets = (card) => {
    return {
      ...card,
      imageUrl: assets.images[card.imageKey] || null,
      content: {
        ...card.content,
        solutionFlow: card.content.solutionFlow.map(flow => assets.solutionFlows[flow] || null),
        demo: assets.videos[card.content.demo] || null,
        techArchitecture: card.content.techArchitecture.map(arch => assets.architectures[arch] || null),
        descriptionFlow: card.content.description.map(desc => assets.descriptions[desc] || null),
        benefitsFlow: card.content.benefits.map(benefit => assets.solutionsBenefits[benefit] || null),
        adoptionFlow: card.content.adoption.map(adopt => assets.adoption[adopt] || null),
      },
    };
  };

  const cardsData = [
    mapAssets(IntelligentAssist),
    mapAssets(EmailEAR),
    mapAssets(CaseIntelligence),
    mapAssets(SmartRecruit),
    // ... other card data
  ];

  return cardsData;
};






import React, { useState, useEffect } from "react";
import { BrowserRouter as Router, Routes, Route, useLocation } from "react-router-dom";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faChevronDown, faChevronUp } from "@fortawesome/free-solid-svg-icons";
import Header from "./components/Header/Header";
import Home from "./Home";
import SideBarPage from "./components/Sidebar/SideBarPage";
import AllCardsPage from "./components/Cards/AllCardsPage";
import { useCardsData } from "./data";
import { BeatLoader } from "react-spinners";
import styles from "./App.module.css";

const MainApp = () => {
  const location = useLocation();
  const [cardsData, setCardsData] = useState([]);
  const [loading, setLoading] = useState(true);
  const [currentIndex, setCurrentIndex] = useState(0);
  const [bigIndex, setBigIndex] = useState(null);
  const [showScrollDown, setShowScrollDown] = useState(true);
  const [showScrollUp, setShowScrollUp] = useState(false);
  const cardsContainerRef = useRef(null);

  const allCardsData = useCardsData();

  useEffect(() => {
    setCardsData(allCardsData);
    setLoading(false);
  }, [allCardsData]);

  const toggleSize = (index) => {
    setBigIndex(index === bigIndex ? null : index);
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

  return (
    <div className={styles.app}>
      {loading ? (
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
