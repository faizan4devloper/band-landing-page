import React, { createContext, useState, useContext } from 'react';

const ThemeContext = createContext();

export const ThemeProvider = ({ children }) => {
  const [theme, setTheme] = useState(localStorage.getItem('theme') || 'light');

  const toggleTheme = () => {
    const newTheme = theme === 'light' ? 'dark' : 'light';
    setTheme(newTheme);
    localStorage.setItem('theme', newTheme);
  };

  return (
    <ThemeContext.Provider value={{ theme, toggleTheme }}>
      {children}
    </ThemeContext.Provider>
  );
};

export const useTheme = () => useContext(ThemeContext);



import React from 'react';
import { BrowserRouter as Router, Routes, Route } from 'react-router-dom';
import { ThemeProvider } from './ThemeContext'; // Import ThemeProvider
import Header from './components/Header/Header';
import Home from './Home';
import SideBarPage from './components/Sidebar/SideBarPage';
import AllCardsPage from './components/Cards/AllCardsPage';
import Chatbot from './components/ChatBot/Chatbot';
import Footer from './components/Footer/Footer';
import { HeaderProvider } from './components/Context/HeaderContext';
import styles from './App.module.css';

const App = () => (
  <Router>
    <ThemeProvider>
      <HeaderProvider>
        <div className={styles.app}>
          <Header />
          <Routes>
            <Route path="/" element={<Home />} />
            <Route path="/dashboard" element={<SideBarPage />} />
            <Route path="/all-cards" element={<AllCardsPage />} />
          </Routes>
          <Chatbot />
          <Footer />
        </div>
      </HeaderProvider>
    </ThemeProvider>
  </Router>
);

export default App;



import React from 'react';
import { useTheme } from '../ThemeContext'; // Import useTheme hook
import Modal from 'react-modal';
import { useNavigate } from 'react-router-dom';
import styles from './Header.module.css';
import logoImage from './HCLTechLogo.svg';
import RequestDemoForm from './RequestDemoForm';
import FeedbackForm from './FeedbackForm'; 
import feedbackImg from './feedback10.svg';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faMoon, faSun } from '@fortawesome/free-solid-svg-icons';

const Header = () => {
  const { theme, toggleTheme } = useTheme(); // Use theme and toggleTheme from context
  const [modalIsOpen, setModalIsOpen] = useState(false);
  const [feedbackModalIsOpen, setFeedbackModalIsOpen] = useState(false);
  const [isVisible, setIsVisible] = useState(true);
  const [lastScrollY, setLastScrollY] = useState(0);
  const navigate = useNavigate();

  const handleImageClick = () => {
    navigate('/');
  };

  const openModal = () => setModalIsOpen(true);
  const closeModal = () => setModalIsOpen(false);
  const openFeedbackModal = () => setFeedbackModalIsOpen(true);
  const closeFeedbackModal = () => setFeedbackModalIsOpen(false);

  const controlHeaderVisibility = () => {
    if (window.scrollY > lastScrollY) {
      setIsVisible(false);
    } else {
      setIsVisible(true);
    }
    setLastScrollY(window.scrollY);
  };

  useEffect(() => {
    window.addEventListener('scroll', controlHeaderVisibility);
    return () => window.removeEventListener('scroll', controlHeaderVisibility);
  }, [lastScrollY]);

  return (
    <div className={`${styles.navbarWrapper} ${isVisible ? styles.show : styles.hide}`} style={{ zIndex: 1000 }}>
      <nav className={styles.header}>
        <div className={styles.logo}>
          <img src={logoImage} alt="" onClick={handleImageClick} title="Navigate to Home" />
        </div>
        <div className={styles.right}>
          <button className={styles.button} onClick={openModal}>
            Request For Live Demo
          </button>
          <img src={feedbackImg} className={`${styles.feedbackImage} ${styles.whiteImage}`} alt="feedback" onClick={openFeedbackModal} title="Provide Feedback"/>          
          <button
            className={styles.themeToggleButton}
            onClick={toggleTheme} // Use toggleTheme from context
            title="Toggle Theme"
          >
            <FontAwesomeIcon
              icon={theme === 'light' ? faMoon : faSun}
              className={styles.themeIcon}
            />
            <span className={styles.themeText}>
              {theme === 'light' ? 'Dark Mode' : 'Light Mode'}
            </span>
          </button>
          <Modal isOpen={modalIsOpen} onRequestClose={closeModal} className={styles.modal}>
            <RequestDemoForm closeModal={closeModal} />
          </Modal>
          <Modal isOpen={feedbackModalIsOpen} onRequestClose={closeFeedbackModal} className={styles.modal}>
            <FeedbackForm closeModal={closeFeedbackModal} />
          </Modal>
        </div>
      </nav>
    </div>
  );
};

export default Header;



import React from 'react';
import { useTheme } from './ThemeContext'; // Import useTheme hook
import styles from './Home.module.css'; // Assume you have separate CSS for themes

const Home = () => {
  const { theme } = useTheme(); // Use theme from context

  return (
    <div className={theme === 'light' ? styles.lightTheme : styles.darkTheme}>
      {/* Your component content */}
    </div>
  );
};

export default Home;
