// AssetImports.js

// Static Images
export const images = {
  IntelligentAssist: require('./components/Cards/CardsImages/card3.jpg'),
  EmailEAR: require('./components/Cards/CardsImages/card1.jpg'),
  CaseIntelligence: require('./components/Cards/CardsImages/card4.jpg'),
  SmartRecruit: require('./components/Cards/CardsImages/card8.jpg'),
  IAssureClaim: require('./components/Cards/CardsImages/card9.jpg'),
  AssistantEV: require('./components/Cards/CardsImages/card10.jpg'),
  CitizenAdvisor: require('./components/Cards/CardsImages/dummy2.jpg'),
  FinanceCompetitor: require('./components/Cards/CardsImages/card82.jpg'),
  Signature: require('./components/Cards/CardsImages/card2.jpg'),
  AIForce: require('./components/Cards/CardsImages/card19.jpg'),
  APICase: require('./components/Cards/CardsImages/card13.jpg'),
  AMSSupport: require('./components/Cards/CardsImages/AUTOMATION.jpg'),
  SOP: require('./components/Cards/CardsImages/SOP.jpg'),
  CodeGReat: require('./components/Cards/CardsImages/card5.jpg'),
  AAIG: require('./components/Cards/CardsImages/card16.jpg'),
  ResponsibleGen: require('./components/Cards/CardsImages/card17.jpg'),
  GraphData: require('./components/Cards/CardsImages/card18.jpg'),
  PredictiveAsset: require('./components/Cards/CardsImages/card11.jpg'),
};

// Static Videos
export const videos = {
  EmailEARDemo: 'https://aiml-convai.s3.amazonaws.com/demovideos/Email-EAR_Demo_new.mp4',
  SignatureExtractionDemo: 'https://aiml-convai.s3.amazonaws.com/demovideos/Sign_Verification_New.mp4',
  IntelligentAssistDemo: 'https://aiml-convai.s3.amazonaws.com/demovideos/Intelligent_Assist-QnA_DemoVideo_new.mp4',
  CaseIntelligenceDemo: 'https://aiml-convai.s3.amazonaws.com/demovideos/Case-Intelligence_demo.mp4',
  CodeGReatDemo: 'https://aiml-convai.s3.amazonaws.com/demovideos/CodeGreat_Demo_new.mp4',
  SmartRecruitDemo: 'https://aiml-convai.s3.amazonaws.com/demovideos/SmartRecruit_IvAssist_Demo.mp4',
  CitizenAdvisorDemo: 'https://aiml-convai.s3.amazonaws.com/demovideos/Citizen_Advisor-Demo1.mp4',
};

// Fetch dynamic assets
export async function fetchDynamicAssets() {
  try {
    const response = await fetch('https://aiml-convai.s3.amazonaws.com/portal-slides/URLJson.json');
    if (!response.ok) {
      throw new Error('Network response was not ok');
    }
    return await response.json();
  } catch (error) {
    console.error('Failed to fetch urldata.json:', error);
    return null;
  }
}

// Retrieve URLs for dynamic assets
export function getDynamicAssetUrl(type, key, urldata) {
  const asset = urldata[type]?.find(item => item.key === key);
  return asset ? asset.url : null;
}





import { images, videos, fetchDynamicAssets, getDynamicAssetUrl } from './AssetImports';

// Static card data
import IntelligentAssist from './CardsData/IntelligentAssist.json';
import CitizenAdvisor from './CardsData/CitizenAdvisor.json';
// Import other card data similarly...

// Function to map assets dynamically
async function getDynamicCardsData() {
  const urldata = await fetchDynamicAssets();
  if (!urldata) return [];

  function mapDynamicAssets(card) {
    return {
      ...card,
      content: {
        ...card.content,
        solutionFlow: Array.isArray(card.content.solutionFlow)
          ? card.content.solutionFlow.map(flow => getDynamicAssetUrl('solutionFlows', flow, urldata))
          : [],
        demo: card.content.demo ? videos[card.content.demo] : null,
        techArchitecture: Array.isArray(card.content.techArchitecture)
          ? card.content.techArchitecture.map(arch => getDynamicAssetUrl('architectures', arch, urldata))
          : [],
        descriptionFlow: Array.isArray(card.content.description)
          ? card.content.description.map(desc => getDynamicAssetUrl('descriptions', desc, urldata))
          : [],
        benefitsFlow: Array.isArray(card.content.benefits)
          ? card.content.benefits.map(benefit => getDynamicAssetUrl('solutionsBenefits', benefit, urldata))
          : [],
        adoptionFlow: Array.isArray(card.content.adoption)
          ? card.content.adoption.map(adopt => getDynamicAssetUrl('adoption', adopt, urldata))
          : [],
      },
    };
  }

  return [
    mapDynamicAssets(IntelligentAssist),
    mapDynamicAssets(CitizenAdvisor),
    // Map other cards similarly...
  ];
}

// Get cards data
export const cardsData = getDynamicCardsData();



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


import React, { useState, useEffect, useRef } from "react";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faArrowRight, faArrowLeft, faPlay, faPause } from "@fortawesome/free-solid-svg-icons";
import MyCarousel from "./components/Carousel/MyCarousel";
import Cards from "./components/Cards/Cards";
import styles from "./App.module.css";
import { Link } from "react-router-dom";
import BgVideo from "./BgVideos1.mp4";

const Home = ({
  cardsData,
  handleClickLeft,
  handleClickRight,
  currentIndex,
  bigIndex,
  toggleSize,
  cardsContainerRef,
  handleMouseEnter,
}) => {
  const [videoState, setVideoState] = useState("hidden");
  const [isPlaying, setIsPlaying] = useState(false);
  const videoRef = useRef(null);

  const handleScroll = () => {
    if (videoRef.current) {
      const videoPosition = videoRef.current.getBoundingClientRect().top;
      const triggerPoint = window.innerHeight / 2;

      if (videoPosition <= triggerPoint && videoState !== "big") {
        setVideoState("big");
        if (!isPlaying) {
          videoRef.current.play();
          setIsPlaying(true);
        }
      } else if (videoPosition > triggerPoint + 100 && videoState !== "small") {
        setVideoState("small");
        if (isPlaying) {
          videoRef.current.pause();
          setIsPlaying(false);
        }
      }
    }
  };

  const togglePlayPause = () => {
    const videoElement = videoRef.current;
    if (videoElement) {
      if (isPlaying) {
        videoElement.pause();
      } else {
        videoElement.play();
      }
      setIsPlaying(!isPlaying);
    } else {
      console.error("Video element is not available or not properly referenced.");
    }
  };

  useEffect(() => {
    window.addEventListener("scroll", handleScroll);

    return () => {
      window.removeEventListener("scroll", handleScroll);
    };
  }, []);

  return (
    <>
      <MyCarousel />
      <div className={`${styles.videoContainer} ${styles[videoState]}`}>
        <video
          className={styles.video}
          src={BgVideo}
          muted
          loop
          ref={videoRef}
          playsInline
          onClick={togglePlayPause} // Allow play/pause by clicking on video
        />
        <button
          className={`${styles.playPauseButton} ${!isPlaying ? "pulse" : ""}`}
          onClick={togglePlayPause}
        >
          <FontAwesomeIcon icon={isPlaying ? faPause : faPlay} />
        </button>
      </div>
      <div
        className={styles.cardsContainer}
        ref={cardsContainerRef}
        onMouseEnter={handleMouseEnter}
      >
        <div className={styles.viewAllContainer}>
          <Link to="/all-cards" className={styles.viewAllButton}>
            View All Solutions <FontAwesomeIcon icon={faArrowRight} className={styles.icon} />
          </Link>
        </div>
        <span className={`${styles.arrow} ${styles.leftArrow}`} onClick={handleClickLeft}>
          <FontAwesomeIcon icon={faArrowLeft} title="Previous" />
        </span>
        {cardsData.slice(currentIndex, currentIndex + 5).map((card, index) => {
          const actualIndex = currentIndex + index;
          return (
            <Cards
              key={index}
              imageUrl={card.imageUrl}
              title={card.title}
              description={card.description}
              isBig={actualIndex === bigIndex}
              toggleSize={() => toggleSize(actualIndex)}
            />
          );
        })}
        <span className={`${styles.arrow} ${styles.rightArrow}`} onClick={handleClickRight}>
          <FontAwesomeIcon icon={faArrowRight} title="Next" />
        </span>
      </div>
    </>
  );
};

export default Home;

{
    
    "solutionFlows": [
      {"key": "CitizenAdvisorFlow1", "url": "https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisor/solutionFlows/CitizenAdvisorFlow1.png"},
      {"key": "SmartRecruitFlow1", "url": "https://aiml-convai.s3.amazonaws.com/portal-slides/smart-recruit/solutionFlows/SmartRecruitSolutionFlow1.png"},
      {"key": "EmailEarFlow", "url": "https://aiml-convai.s3.amazonaws.com/portal-slides/emailear/solutionFlow/Slide2.JPG"}
    ],
    "architectures": [
      {"key": "CitizenAdvisorArchitecture", "url": "https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisor/technical-architecture/CitizenAdvisorArchitecture.png"},
      {"key": "SmartRecruitArchitecture", "url": "https://aiml-convai.s3.amazonaws.com/portal-slides/smart-recruit/technical-architecture/SmartRecruitArchitecture.png"},
      {"key": "EmailEARArchitecture", "url": "https://aiml-convai.s3.amazonaws.com/portal-slides/emailear/techArchitecture/Slide3.JPG"}
    ],
    "descriptions": [
      {"key": "CitizenAdvisorDescription", "url": "https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisor/description/CitizenAdvisorDescription.png"},
      {"key": "SmartRecruitDescription1", "url": "https://aiml-convai.s3.amazonaws.com/portal-slides/smart-recruit/description/SmartRecruitDescription1.png"},
      {"key": "EmailEARDemo", "url": "https://aiml-convai.s3.amazonaws.com/portal-slides/emailear/description/Slide1.JPG"}
    ],
    "solutionsBenefits": [
      {"key": "CitizenAdvisorBenefits", "url": "https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisor/benefits/CitizenAdvisorBenefits.png"},
      {"key": "SmartRecruitBenefits", "url": "https://aiml-convai.s3.amazonaws.com/portal-slides/smart-recruit/benefits/SmartRecruitBenefits.png"},
      {"key": "EmailEARBenefits", "url": "https://aiml-convai.s3.amazonaws.com/portal-slides/emailear/benefits/Slide4.JPG"}
    ],
    "adoption": [
      {"key": "CitizenAdvisorAdoption", "url": "https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisor/industry-adoption/CitizenAdvisorAdoption.png"},
      {"key": "SmartRecruitAdoption", "url": "https://aiml-convai.s3.amazonaws.com/portal-slides/smart-recruit/industry-adoption/SmartRecruitAdoption.png"},
      {"key": "EmailEARAdoption", "url": "https://aiml-convai.s3.amazonaws.com/portal-slides/emailear/adoption/Slide5.JPG"}
    ]
  }
  
  
  
  
