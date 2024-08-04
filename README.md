ERROR in ./src/App.js 29:25-39
export 'cardsData' (imported as 'fetchCardsData') was not found in './data' (possible exports: getCardsData)
ERROR in ./src/components/Sidebar/SideBarPage.js 23:25-39
export 'cardsData' (imported as 'fetchCardsData') was not found in '../../data' (possible exports: getCardsData)


import React, { useState, useEffect } from 'react';
import { useLocation } from 'react-router-dom';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faArrowLeft } from '@fortawesome/free-solid-svg-icons';
import Header from '../Header/Header';
import SideBar from './SideBar';
import MainContent from './MainContent';
import styles from './SideBarPage.module.css';
import { cardsData as fetchCardsData } from '../../data';

const SideBarPage = () => {
  const [activeTab, setActiveTab] = useState('description');
  const [cardContent, setCardContent] = useState({});
  const [cardTitle, setCardTitle] = useState('');
  const location = useLocation();

  useEffect(() => {
    const fetchData = async () => {
      const data = await fetchCardsData;
      const params = new URLSearchParams(location.search);
      const title = params.get('title');
      if (title) {
        setCardTitle(title);
        const card = data.find((c) => c.title === title);
        if (card) {
          setCardContent(card.content);
        }
      }
    };

    fetchData();
  }, [location.search]);

  const handleTabChange = (tabId) => {
    setActiveTab(tabId);
  };

  const handleBackButtonClick = () => {
    window.history.back();
  };

  return (
    <div className={styles.sideBarPage}>
      <Header />
      <div className={styles.header2}>
        <button onClick={handleBackButtonClick} className={styles.backButton}>
          <FontAwesomeIcon icon={faArrowLeft} />
        </button>
        {cardTitle && <div className={styles.cardTitle}>{cardTitle}</div>}
      </div>
      <div className={styles.contentWrapper}>
        <SideBar activeTab={activeTab} handleTabChange={handleTabChange} />
        <MainContent activeTab={activeTab} content={cardContent} />
      </div>
    </div>
  );
};

export default SideBarPage;




import React, { useState, useEffect, useRef } from 'react';
import { BrowserRouter as Router, Routes, Route, useLocation } from 'react-router-dom';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faChevronDown, faChevronUp } from '@fortawesome/free-solid-svg-icons';
import Header from './components/Header/Header';
import Home from './Home';
import SideBarPage from './components/Sidebar/SideBarPage';
import AllCardsPage from './components/Cards/AllCardsPage';
import { cardsData as fetchCardsData } from './data';
import { BeatLoader } from 'react-spinners';
import styles from './App.module.css';

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
      const data = await fetchCardsData;
      setCardsData(data);
      setLoading(false); // Set loading to false after data is fetched
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
      cardsContainerRef.current.scrollIntoView({ behavior: 'smooth' });
      setShowScrollDown(false);
      setShowScrollUp(true);
    }
  };

  const handleScrollUp = () => {
    window.scrollTo({ top: 0, behavior: 'smooth' });
    setShowScrollDown(true);
    setShowScrollUp(false);
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
    window.addEventListener('scroll', debounceScroll);

    return () => window.removeEventListener('scroll', debounceScroll);
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
                />
              }
            />
            <Route path="/dashboard" element={<SideBarPage />} />
            <Route
              path="/all-cards"
              element={<AllCardsPage cardsData={cardsData} cardsContainerRef={cardsContainerRef} />}
            />
          </Routes>
          {showScrollDown && location.pathname !== '/all-cards' && location.pathname !== '/dashboard' && (
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
