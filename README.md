// AssetImports.js
import axios from 'axios';
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

// Videos
export const videos = {
  EmailEARDemo: 'https://aiml-convai.s3.amazonaws.com/demovideos/Email-EAR_Demo_new.mp4',
  SignatureExtractionDemo: 'https://aiml-convai.s3.amazonaws.com/demovideos/Sign_Verification_New.mp4',
  IntelligentAssistDemo: 'https://aiml-convai.s3.amazonaws.com/demovideos/Intelligent_Assist-QnA_DemoVideo_new.mp4',
  CaseIntelligenceDemo: 'https://aiml-convai.s3.amazonaws.com/demovideos/Case-Intelligence_demo.mp4',
  CodeGReatDemo: 'https://aiml-convai.s3.amazonaws.com/demovideos/CodeGreat_Demo_new.mp4',
  SmartRecruitDemo: 'https://aiml-convai.s3.amazonaws.com/demovideos/SmartRecruit_IvAssist_Demo.mp4',
  CitizenAdvisorDemo: 'https://aiml-convai.s3.amazonaws.com/demovideos/Citizen_Advisor-Demo1.mp4',
};


// Fetch and cache JSON data using Axios
export async function fetchAssets() {
  try {
    const response = await axios.get('https://aiml-convai.s3.amazonaws.com/portal-slides/urldata.json');
    return response.data;
  } catch (error) {
    console.error('Failed to fetch assets:', error);
    return {}; // Fallback to an empty object in case of error
  }
}


// data.js
import IntelligentAssist from './CardsData/IntelligentAssist.json';
import EmailEAR from './CardsData/EmailEAR.json';
import CaseIntelligence from './CardsData/CaseIntelligence.json';
import SmartRecruit from './CardsData/SmartRecruit.json';
import IAssureClaim from './CardsData/IAssureClaim.json';
import AssistantEV from './CardsData/AssistantEV.json';
import AutoWiseCompanion from './CardsData/AutoWiseCompanion.json';
import CitizenAdvisor from './CardsData/CitizenAdvisor.json';
import FinCompetitor from './CardsData/FinCompetitor.json';
import SignatureExtraction from './CardsData/SignatureExtraction.json';
import AiForce from './CardsData/AiForce.json';
import ApiCase from './CardsData/ApiCase.json';
import AmsSupport from './CardsData/AmsSupport.json';
import CodeGreat from './CardsData/CodeGreat.json';
import AaigApi from './CardsData/AaigApi.json';
import ResponsibleGen from './CardsData/ResponsibleGen.json';
import GraphData from './CardsData/GraphData.json';
import PredictiveAsset from './CardsData/PredictiveAsset.json';


import { images, videos, fetchAssets } from './AssetImports';

async function mapAssets(card) {
  const assets = await fetchAssets();
  const assetKey = card.title.replace(/\s+/g, '');

  const data = assets[`${assetKey}description`] || {};

  return {
    ...card,
    imageUrl: images[card.imageUrl] || 'defaultImagePath.jpg', // Fallback to a default image
    content: {
      ...card.content,
      solutionFlow: data.solutionFlow || null,
      demo: videos[card.content.demo] || null,
      techArchitecture: data.techArchitecture || null,
      descriptionFlow: data.description || null,
      benefitsFlow: data.benefits || null,
      adoption: data.adoption || null,
    },
  };
}

// Mapping cards data asynchronously
export async function getCardsData() {
  const cardsData = await Promise.all([
    mapAssets(IntelligentAssist),
    mapAssets(EmailEAR),
    mapAssets(CaseIntelligence),
    mapAssets(CitizenAdvisor)
    // ... other cards
  ]);

  return cardsData;
}


{
  "imageUrl": "CitizenAdvisor", 
  "title": "Citizen Advisor",
  "description": "An experience transformation from disconnected silos of information to intuitive, personalized revelations.",
  "industry": "GOVT",
  "businessFunction": "Customer Experience",
  "content": {
    "description": "CitizenAdvisordescription",
    "solutionFlow": "CitizenAdvisorsolutionFlow",
    "demo": "CitizenAdvisorDemo",
    "techArchitecture": "CitizenAdvisortechArchitecture",
    "benefits": "CitizenAdvisorbenefits",
    "adoption": "CitizenAdvisoradoption"
  }
}

import React, { useState, useEffect, useRef } from 'react';
import { BrowserRouter as Router, Routes, Route, useLocation } from 'react-router-dom';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faChevronDown, faChevronUp } from '@fortawesome/free-solid-svg-icons';
import Header from './components/Header/Header';
import Home from './Home';
import SideBarPage from './components/Sidebar/SideBarPage';
import AllCardsPage from './components/Cards/AllCardsPage';
import { getCardsData } from './data';
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
      const data = await getCardsData();
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



import React, { useState } from "react";
import { Carousel } from "react-responsive-carousel";
import "react-responsive-carousel/lib/styles/carousel.min.css"; // Import carousel styles
import styles from "./MainContent.module.css";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faTimes } from "@fortawesome/free-solid-svg-icons";
import Video from "./Video";

const MainContent = ({ activeTab, content }) => {
  const [maximizedImage, setMaximizedImage] = useState(null);
  const [currentSlide, setCurrentSlide] = useState(0);

  const toggleMaximize = (imageSrc) => {
    setMaximizedImage(maximizedImage === imageSrc ? null : imageSrc);
  };

  const renderCarousel = (images) => (
    <div className={styles.carouselContainer}>
      <div className={styles.customThumbs}>
        {images.map((image, index) => (
          <div
            key={index}
            className={`${styles.customThumbContainer} ${currentSlide === index ? styles.selected : ""}`}
            onClick={() => setCurrentSlide(index)}
          >
            <img src={image} alt={`Thumbnail ${index + 1}`} className={styles.customThumb} />
          </div>
        ))}
      </div>
      <Carousel
        showArrows={false}
        showIndicators={false}
        showThumbs={false}
        showStatus={false}
        selectedItem={currentSlide}
        onChange={(index) => setCurrentSlide(index)}
        className={styles.customCarousel}
      >
        {images.map((image, index) => (
          <div key={index}>
            <img
              src={image}
              alt={`Slide ${index + 1}`}
              className={maximizedImage === image ? styles.maximized : ""}
              onClick={() => toggleMaximize(image)}
            />
          </div>
        ))}
      </Carousel>
    </div>
  );

  const renderImageOrCarousel = (images) => {
    if (!images || images.length === 0) {
      return <div>No images available</div>;
    }
    
    return images.length > 1 ? renderCarousel(images) : (
      <img
        src={images[0]}
        alt="Single Image"
        className={maximizedImage === images[0] ? styles.maximized : ""}
        onClick={() => toggleMaximize(images[0])}
      />
    );
  };

  if (!content) {
    return <div className={styles.mainContent}>Content not available</div>;
  }

  const contentMap = {
    description: (
      <div className={styles.description}>
        {renderImageOrCarousel(content.descriptionFlow)}
      </div>
    ),
    solutionFlow: (
      <div className={styles.solution}>
        {renderImageOrCarousel(content.solutionFlow)}
      </div>
    ),
    demo: (
      <div className={styles.demo}>
        <Video src={content.demo} />
      </div>
    ),
    techArchitecture: (
      <div className={styles.architecture}>
        {renderImageOrCarousel(content.techArchitecture)}
      </div>
    ),
    benefits: (
      <div className={styles.benefits}>
        {renderImageOrCarousel(content.benefitsFlow)}
      </div>
    ),
    adoption: (
      <div className={styles.adoption}>
        {renderImageOrCarousel(content.adoptionFlow)}
      </div>
    ),
  };

  return (
    <div className={styles.mainContent}>
      {contentMap[activeTab] || <div>Content not available</div>}
      {maximizedImage && (
        <div className={styles.overlay} onClick={() => setMaximizedImage(null)}>
          <FontAwesomeIcon icon={faTimes} className={styles.closeIcon} onClick={() => setMaximizedImage(null)} />
          <img src={maximizedImage} alt="Maximized view" className={styles.maximized} />
        </div>
      )}
    </div>
  );
};

export default MainContent;

// urldata.json from s3
{"CitizenAdvisorbenefits": ["https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisorv1/benefits/Slide8.JPG"], "CitizenAdvisortechArchitecture": ["https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisorv1/techArchitecture/Slide7.JPG"], "CitizenAdvisorsolutionFlow": ["https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisorv1/solutionFlow/Slide6.JPG", "https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisorv1/solutionFlow/Slide3.JPG", "https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisorv1/solutionFlow/Slide5.JPG", "https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisorv1/solutionFlow/Slide2.JPG", "https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisorv1/solutionFlow/Slide4.JPG"], "CitizenAdvisordescription": ["https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisorv1/description/Slide1.JPG"], "CitizenAdvisoradoption": ["https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisorv1/adoption/Slide9.JPG"], "CaseIntelligencedescription": ["https://aiml-convai.s3.amazonaws.com/portal-slides/caseintelligence/description/Slide1.JPG"], "CaseIntelligencetechArchitecture": ["https://aiml-convai.s3.amazonaws.com/portal-slides/caseintelligence/techArchitecture/Slide3.JPG"], "CaseIntelligenceadoption": ["https://aiml-convai.s3.amazonaws.com/portal-slides/caseintelligence/adoption/Slide5.JPG"], "CaseIntelligencesolutionFlow": ["https://aiml-convai.s3.amazonaws.com/portal-slides/caseintelligence/solutionFlow/Slide2.JPG"], "CaseIntelligencebenefits": ["https://aiml-convai.s3.amazonaws.com/portal-slides/caseintelligence/benefits/Slide4.JPG"], "IntelligentAssistdescription": ["https://aiml-convai.s3.amazonaws.com/portal-slides/intelligentassist/description/Slide1.JPG"], "IntelligentAssisttechArchitecture": ["https://aiml-convai.s3.amazonaws.com/portal-slides/intelligentassist/techArchitecture/Slide3.JPG"], "IntelligentAssistadoption": ["https://aiml-convai.s3.amazonaws.com/portal-slides/intelligentassist/adoption/Slide5.JPG"], "IntelligentAssistsolutionFlow": ["https://aiml-convai.s3.amazonaws.com/portal-slides/intelligentassist/solutionFlow/Slide2.JPG"], "IntelligentAssistbenefits": ["https://aiml-convai.s3.amazonaws.com/portal-slides/intelligentassist/benefits/Slide4.JPG"], "SmartRecruitadoption": ["https://aiml-convai.s3.amazonaws.com/portal-slides/smartrecruit/adoption/Slide7.JPG"], "SmartRecruitbenefits": ["https://aiml-convai.s3.amazonaws.com/portal-slides/smartrecruit/benefits/Slide6.JPG"], "SmartRecruitdescription": ["https://aiml-convai.s3.amazonaws.com/portal-slides/smartrecruit/description/Slide1.JPG", "https://aiml-convai.s3.amazonaws.com/portal-slides/smartrecruit/description/Slide2.JPG"], "SmartRecruitsolutionFlow": ["https://aiml-convai.s3.amazonaws.com/portal-slides/smartrecruit/solutionFlow/Slide3.JPG", "https://aiml-convai.s3.amazonaws.com/portal-slides/smartrecruit/solutionFlow/Slide4.JPG"], "SmartRecruittechArchitecture": ["https://aiml-convai.s3.amazonaws.com/portal-slides/smartrecruit/techArchitecture/Slide5.JPG"], "EmaiEardescription": ["https://aiml-convai.s3.amazonaws.com/portal-slides/emailear/description/Slide1.JPG"], "EmaiEartechArchitecture": ["https://aiml-convai.s3.amazonaws.com/portal-slides/emailear/techArchitecture/Slide3.JPG"], "EmaiEaradoption": ["https://aiml-convai.s3.amazonaws.com/portal-slides/emailear/adoption/Slide5.JPG"], "EmaiEarsolutionFlow": ["https://aiml-convai.s3.amazonaws.com/portal-slides/emailear/solutionFlow/Slide2.JPG"], "EmaiEarbenefits": ["https://aiml-convai.s3.amazonaws.com/portal-slides/emailear/benefits/Slide4.JPG"]}

// CitizenAdvisor.json
{
  "imageUrl": "CitizenAdvisor", 
  "title": "Citizen Advisor",
  "description": "An experience transformation from disconnected silos of information to intuitive, personalized revelations.",
  "industry": "GOVT",
  "businessFunction": "Customer Experience",
  "content": {
    "description": "CitizenAdvisordescription",
    "solutionFlow": "CitizenAdvisorsolutionFlow",
    "demo": "CitizenAdvisorDemo",
    "techArchitecture": "CitizenAdvisortechArchitecture",
    "benefits": "CitizenAdvisorbenefits",
    "adoption": "CitizenAdvisoradoption"
  }
}
