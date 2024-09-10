.chatbotIcon {
  position: fixed;
  bottom: 20px;
  right: 20px;
background: linear-gradient(90deg, #6f36cd 0%, #1f77f6 100%);  color: white;
  padding: 10px;
  border-radius: 50%;
  cursor: pointer;
  z-index: 1000;
  animation: float 3s ease-in-out infinite;
}

@keyframes float {
  0%, 100% {
    transform: translateY(0);
  }
  50% {
    transform: translateY(-10px);
  }
}

.chatbotContainer {
 position: fixed;
  bottom: 80px;
  right: 20px;
  width: 320px;
  height: 420px;
  background-color: white;
  border-radius: 8px;
  box-shadow: 0 0 15px rgba(0, 0, 0, 0.3);
  display: flex;
  flex-direction: column;
  z-index: 1001;
  opacity: 0;
  transform: scale(0.9);
  transition: opacity 0.3s ease, transform 0.3s ease;
}

.chatbotContainer.open {
  opacity: 1;
  transform: scale(1);
}

.chatbotHeader {
  display: flex;
  justify-content: space-between;
  align-items: center;
background: linear-gradient(90deg, #6f36cd 0%, #1f77f6 100%);  color: white;
  padding: 10px;
  border-top-left-radius: 8px;
  border-top-right-radius: 8px;
}

.chatbotTitle {
  font-size: 16px;
  font-weight: bold;
}

.closeButton, .clearChatButton {
  background: none;
  border: none;
  color: white;
  font-size: 16px;
  cursor: pointer;
}

.chatbotMessages {
  flex: 1;
  padding: 10px;
  overflow-y: auto;
  display: flex;
  flex-direction: column;
  animation: slideIn 0.5s ease;
}

@keyframes slideIn {
  from {
    opacity: 0;
    transform: translateY(10px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.userMessage, .botMessage {
  display: flex;
  align-items: center;
  margin-bottom: 10px;
}

.userMessage {
  justify-content: flex-end;
}

.botMessage {
  justify-content: flex-start;
}

.icon {
  margin: 0 8px;
  font-size: 15px;
}

.messageText {
  max-width: 75%;
  background-color: #f1f1f1;
  padding: 10px;
  font-size: 12px;
  border-radius: 10px;
  color: #333;
  word-wrap: break-word;
  white-space: pre-wrap;
}

.messageText a {
  color: #3f1ec2;
  text-decoration: none;
  font-size: 10px;
  margin-bottom: 5px;
}

.userMessage .messageText {
  background-color: #d1e7ff;
}

.chatbotInput {
  display: flex;
  border-top: 1px solid #ddd;
  padding: 10px;
}

.chatbotInput input {
  flex: 1;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
  outline: none;
}

.chatbotInput button {
background: linear-gradient(90deg, #6f36cd 0%, #1f77f6 100%);  color: white;
  border: none;
  padding: 10px;
  border-radius: 4px;
  margin-left: 10px;
  cursor: pointer;
}

.clearChatOverlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1002;
}

.clearChatWindow {
  position: absolute;
  bottom: 130px; /* Adjust based on available space */
  left: 0;
  right: 0;
  background: white;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 0 15px rgba(0, 0, 0, 0.3);
  text-align: center;
}
.confirmButton {
  background-color: #d9534f;
  color: white;
  padding: 8px 18px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  margin-right: 10px;
}

.cancelButton {
  background-color: #5f1ec1;
  color: white;
  padding: 8px 18px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}



/* ... previous styles ... */

.botProfile {
  display: flex;
  align-items: center;
}

.botImage {
  width: 35px;
  height: 35px;
  border-radius: 50%;
  margin-right: 10px;
}

.botInfo {
  display: flex;
  flex-direction: column;
}



.botStatus {
  font-size: 12px;
  color: #d1e7ff;
}

/* Rest of the styles */
.headerActions {
  display: flex;
  align-items: center;
}

.closeButton, .clearChatButton {
  background: none;
  border: none;
  color: white;
  font-size: 16px;
  cursor: pointer;
  margin-left: 5px;
}

/* Add this to your existing styles */
.greenDot {
  display: inline-block;
  width: 8px;
  height: 8px;
  background-color: #28a745; /* Green color */
  border-radius: 50%;
  margin-right: 4px;
}

/* Ensure the botStatus class is updated */
.botStatus {
  font-size: 12px;
  color: #d1e7ff;
  display: flex;
  align-items: center;
}

.onlineDot {
  width: 8px;
  height: 8px;
  background-color: #4caf50; /* Green dot */
  border-radius: 50%;
  display: inline-block;
  margin-right: 5px;
}

.minimizeButton {
  background: none;
  border: none;
  color: white;
  font-size: 16px;
  cursor: pointer;
  margin-left: 5px;
}

/* Add tooltip styles */
.clearChatButton[title], .minimizeButton[title] {
  position: relative;
}
















import React, { useState, useEffect, useRef } from 'react';
import { BrowserRouter as Router, Routes, Route, useLocation } from 'react-router-dom';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faChevronDown, faChevronUp, faMoon, faSun } from '@fortawesome/free-solid-svg-icons';
import Header from './components/Header/Header';
import Home from './Home';
import SideBarPage from './components/Sidebar/SideBarPage';
import AllCardsPage from './components/Cards/AllCardsPage';
import Chatbot from './components/ChatBot/Chatbot';
import Footer from './components/Footer/Footer';
import { getCardsData } from './data';
import { BeatLoader } from 'react-spinners';
import { HeaderProvider } from './components/Context/HeaderContext';
import styles from './App.module.css';

const MainApp = () => {
  const location = useLocation();
  const [cardsData, setCardsData] = useState([]);
  const [currentIndex, setCurrentIndex] = useState(0);
  const [bigIndex, setBigIndex] = useState(null);
  const [showScrollDown, setShowScrollDown] = useState(true);
  const [showScrollUp, setShowScrollUp] = useState(false);
  const [loading, setLoading] = useState(true);
  const [theme, setTheme] = useState(localStorage.getItem('theme') || 'light');
  const cardsContainerRef = useRef(null);

  useEffect(() => {
    const fetchData = async () => {
      const data = await getCardsData();
      setCardsData(data);
      setLoading(false);
    };

    fetchData();
  }, []);

  useEffect(() => {
    document.documentElement.setAttribute('data-theme', theme);
    localStorage.setItem('theme', theme);
  }, [theme]);

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

  const showFooter = location.pathname === '/';

  return (
    <HeaderProvider>
      <div className={styles.app}>
        {loading ? (
          <div className={styles.loader}>
            <BeatLoader color="#5931d5" loading={loading} size={15} margin={2} />
          </div>
        ) : (
          <>
            <Header theme={theme} setTheme={setTheme} />
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
            <Chatbot />
            {showFooter && <Footer />}
            <button
              className={styles.themeToggleButton}
              onClick={() => setTheme(theme === 'light' ? 'dark' : 'light')}
            >
              <FontAwesomeIcon
                icon={theme === 'light' ? faMoon : faSun}
                className={styles.themeIcon}
              />
              <span className={styles.themeText}>
                {theme === 'light' ? 'Dark Mode' : 'Light Mode'}
              </span>
            </button>
          </>
        )}
      </div>
    </HeaderProvider>
  );
};

const App = () => (
  <Router>
    <MainApp />
  </Router>
);

export default App;
