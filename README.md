// fetchData.js
export async function fetchUrlData() {
  const response = await fetch('https://aiml-convai.s3.amazonaws.com/urldata.json');
  if (!response.ok) {
    throw new Error('Failed to fetch URL data');
  }
  return response.json();
}






import { fetchUrlData } from './fetchData'; // Make sure to import your data fetching function

const mapAssets = (card, urlData) => {
  const getAssetUrl = (key, type) => urlData[`${card.title}${type}`]?.[0] || null;

  return {
    ...card,
    imageUrl: card.imageUrl ? urlData[card.imageUrl]?.[0] : null,
    content: {
      ...card.content,
      solutionFlow: Array.isArray(card.content.solutionFlow)
        ? card.content.solutionFlow.map(flow => getAssetUrl(flow, 'solutionFlow'))
        : [],
      demo: card.content.demo ? urlData[card.content.demo]?.[0] : null,
      techArchitecture: Array.isArray(card.content.techArchitecture)
        ? card.content.techArchitecture.map(arch => getAssetUrl(arch, 'techArchitecture'))
        : [],
      descriptionFlow: Array.isArray(card.content.description)
        ? card.content.description.map(desc => getAssetUrl(desc, 'description'))
        : [],
      benefitsFlow: Array.isArray(card.content.benefits)
        ? card.content.benefits.map(benefit => getAssetUrl(benefit, 'benefits'))
        : [],
      adoptionFlow: Array.isArray(card.content.adoption)
        ? card.content.adoption.map(adopt => getAssetUrl(adopt, 'adoption'))
        : [],
    },
  };
};

export const loadCardsData = async () => {
  try {
    const urlData = await fetchUrlData();
    const cardDataPromises = [
      import('./CardsData/IntelligentAssist.json'),
      import('./CardsData/EmailEAR.json'),
      import('./CardsData/CaseIntelligence.json'),
      import('./CardsData/SmartRecruit.json'),
      import('./CardsData/IAssureClaim.json'),
      import('./CardsData/AssistantEV.json'),
      import('./CardsData/AutoWiseCompanion.json'),
      import('./CardsData/CitizenAdvisor.json'),
      import('./CardsData/FinCompetitor.json'),
      import('./CardsData/SignatureExtraction.json'),
      import('./CardsData/AiForce.json'),
      import('./CardsData/ApiCase.json'),
      import('./CardsData/AmsSupport.json'),
      import('./CardsData/CodeGreat.json'),
      import('./CardsData/AaigApi.json'),
      import('./CardsData/ResponsibleGen.json'),
      import('./CardsData/GraphData.json'),
      import('./CardsData/PredictiveAsset.json'),
    ];

    const cardData = await Promise.all(cardDataPromises);

    return cardData.map(data => mapAssets(data.default, urlData));
  } catch (error) {
    console.error('Error loading card data:', error);
    return []; // Return an empty array in case of an error
  }
};



import React, { useRef, useState, useEffect } from "react";
import { BrowserRouter as Router, Routes, Route, useLocation } from "react-router-dom";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faArrowRight, faArrowLeft, faChevronDown, faChevronUp } from "@fortawesome/free-solid-svg-icons";
import Header from "./components/Header/Header";
import Home from "./Home";
import SideBarPage from "./components/Sidebar/SideBarPage";
import AllCardsPage from "./components/Cards/AllCardsPage";
import { loadCardsData } from "./data"; // Updated import
import { BeatLoader } from "react-spinners";
import styles from "./App.module.css";

const MainApp = () => {
  const location = useLocation();
  const [cardsData, setCardsData] = useState([]);
  const [currentIndex, setCurrentIndex] = useState(0);
  const [bigIndex, setBigIndex] = useState(null);
  const [showScrollDown, setShowScrollDown] = useState(true);
  const [showScrollUp, setShowScrollUp] = useState(false);
  const [loading, setLoading] = useState(true);
  const cardsContainerRef = useRef(null);

  useEffect(() => {
    const fetchData = async () => {
      const data = await loadCardsData();
      setCardsData(data);
      setLoading(false);
    };

    fetchData();
  }, []);

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

const

