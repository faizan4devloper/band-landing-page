// Images
export const images = {
  IntelligentAss: require('./components/Cards/CardsImages/card3.jpg'),
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

// Solution Flows
export const solutionFlows = {
  EmailEarFlow: require('./components/Sidebar/Icons/EmailEarFlowGraph.png'),
  IntelligentAssistFlow: require('./components/Sidebar/Icons/IntelligentAssistFlowGraph.png'),
  CaseIntelligenceFlow: require('./components/Sidebar/Icons/CaseIntelligenceFlowGraph.png'),
  IAssureClaimFlow: require('./components/Sidebar/Icons/IAssureClaimFlowGraph.png'),
  CitizenAdvisorFlow1: 'https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisor/solutionFlows/CitizenAdvisorFlow1.png',
  CitizenAdvisorFlow2: 'https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisor/solutionFlows/CitizenAdvisorFlow2.png',
  CitizenAdvisorFlow3: 'https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisor/solutionFlows/CitizenAdvisorFlow3.png',
  CitizenAdvisorFlow4: 'https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisor/solutionFlows/CitizenAdvisorFlow4.png',
  CitizenAdvisorFlow5: 'https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisor/solutionFlows/CitizenAdvisorFlow5.png',
  SmartRecruitFlow1: 'https://aiml-convai.s3.amazonaws.com/portal-slides/smart-recruit/solutionFlows/SmartRecruitSolutionFlow1.png',
  SmartRecruitFlow2: 'https://aiml-convai.s3.amazonaws.com/portal-slides/smart-recruit/solutionFlows/SmartRecruitSolutionFlow2.png',
};

// Technical Architectures
export const architectures = {
  IntelligentAssistArchitecture: require('./components/Sidebar/Icons/IntelligentAssistarchitecture.png'),
  EmailEARArchitecture: require('./components/Sidebar/Icons/EmailEARarchitecture.png'),
  CaseIntelligenceArchitecture: require('./components/Sidebar/Icons/CaseIntelligencearchitecture.png'),
  SmartRecruitArchitecture: 'https://aiml-convai.s3.amazonaws.com/portal-slides/smart-recruit/technical-architecture/SmartRecruitArchitecture.png',
  AssistantEvArchitecture: require('./components/Sidebar/Icons/AssistantEvachitecture.png'),
  IAssureClaimArchitecture: require('./components/Sidebar/Icons/IAssureClaimarchitecture.png'),
  AIForceArchitecture: require('./components/Sidebar/Icons/AIForcearchitecture.png'),
  CitizenAdvisorArchitecture: 'https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisor/technical-architecture/CitizenAdvisorArchitecture.png',
};

// Descriptions
export const descriptions = {
  citizenDescription: 'https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisor/description/CitizenAdvisorDescription.png',
  smartRecruitDescription1: 'https://aiml-convai.s3.amazonaws.com/portal-slides/smart-recruit/description/SmartRecruitDescription1.png',
  smartRecruitDescription2: 'https://aiml-convai.s3.amazonaws.com/portal-slides/smart-recruit/description/SmartRecruitDescription2.png',
};

// Solutions Benefits
export const solutionsBenefits = {
  citizenBenefits: 'https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisor/benefits/CitizenAdvisorBenefits.png',
  smartRecruitBenefits: 'https://aiml-convai.s3.amazonaws.com/portal-slides/smart-recruit/benefits/SmartRecruitBenefits.png',
};

// Adoption
export const adoption = {
  citizenAdoption: 'https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisor/industry-adoption/CitizenAdvisorAdoption.png',
  smartRecruitAdoption: 'https://aiml-convai.s3.amazonaws.com/portal-slides/smart-recruit/industry-adoption/SmartRecruitAdoption.png',
};






import {
  images,
  videos,
  solutionFlows,
  architectures,
  descriptions,
  solutionsBenefits,
  adoption
} from './AssetImports';

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


function mapAssets(card) {
  const getImage = (key) => (typeof key === 'string' ? images[key.split('.').pop()] : null);
  const getVideo = (key) => (typeof key === 'string' ? videos[key.split('.').pop()] : null);
  const getSolutionFlows = (keys) => Array.isArray(keys) ? keys.map(key => (typeof key === 'string' ? solutionFlows[key.split('.').pop()] : null)) : (typeof keys === 'string' ? [solutionFlows[keys.split('.').pop()]] : []);
  const getArchitectures = (keys) => Array.isArray(keys) ? keys.map(key => (typeof key === 'string' ? architectures[key.split('.').pop()] : null)) : (typeof keys === 'string' ? [architectures[keys.split('.').pop()]] : []);
  const getDescriptions = (keys) => Array.isArray(keys) ? keys.map(key => (typeof key === 'string' ? descriptions[key.split('.').pop()] : null)) : (typeof keys === 'string' ? [descriptions[keys.split('.').pop()]] : []);
  const getBenefits = (keys) => Array.isArray(keys) ? keys.map(key => (typeof key === 'string' ? solutionsBenefits[key.split('.').pop()] : null)) : (typeof keys === 'string' ? [solutionsBenefits[keys.split('.').pop()]] : []);
  const getAdoption = (keys) => Array.isArray(keys) ? keys.map(key => (typeof key === 'string' ? adoption[key.split('.').pop()] : null)) : (typeof keys === 'string' ? [adoption[keys.split('.').pop()]] : []);

  return {
    ...card,
    imageUrl: getImage(card.imageUrl),
    content: {
      ...card.content,
      solutionFlow: getSolutionFlows(card.content.solutionFlow),
      demo: [getVideo(card.content.demo)], // Ensure demo is always an array
      techArchitecture: getArchitectures(card.content.techArchitecture),
      descriptionFlow: getDescriptions(card.content.description),
      benefitsFlow: getBenefits(card.content.benefits),
      adoptionFlow: getAdoption(card.content.adoption),
    },
  };
}


export const cardsData = [
  mapAssets(IntelligentAssist),
  mapAssets(EmailEAR),
  mapAssets(CaseIntelligence),
  mapAssets(SmartRecruit),
  mapAssets(IAssureClaim),
  mapAssets(AssistantEV),
  mapAssets(AutoWiseCompanion),
  mapAssets(CitizenAdvisor),
  mapAssets(FinCompetitor),
  mapAssets(SignatureExtraction),
  mapAssets(AiForce),
  mapAssets(ApiCase),
  mapAssets(AmsSupport),
  mapAssets(CodeGreat),
  mapAssets(AaigApi),
  mapAssets(ResponsibleGen),
  mapAssets(GraphData),
  mapAssets(PredictiveAsset),
];



{
  "imageUrl": "CitizenAdvisor",
  "title": "Citizen Advisor",
  "description": "An experience transformation from disconnected silos information to an intuitive, personalized revelations",
  "industry": "GOVT",
  "businessFunction": "Customer Experience",
  "content": {
    "description": ["citizenDescription"],
    "solutionFlow": ["CitizenAdvisorFlow1", "CitizenAdvisorFlow2", "CitizenAdvisorFlow3", "CitizenAdvisorFlow4", "CitizenAdvisorFlow5"],
    "demo": "CitizenAdvisorDemo",
    "techArchitecture": ["CitizenAdvisorArchitecture"],
    "benefits": ["citizenBenefits"],
    "adoption": ["citizenAdoption"]
  }
}



import React, { useRef, useState, useEffect } from "react";
import { BrowserRouter as Router, Routes, Route, Link, useLocation } from "react-router-dom";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faArrowRight, faArrowLeft, faChevronDown, faChevronUp } from "@fortawesome/free-solid-svg-icons";
import Header from "./components/Header/Header";
import Home from "./Home"; // Import the updated Home component
import SideBarPage from "./components/Sidebar/SideBarPage";
import AllCardsPage from "./components/Cards/AllCardsPage";
import { cardsData as initialCardsData } from "./data";
import { BeatLoader } from "react-spinners"; // Import loaders
import styles from "./App.module.css"; // Ensure this is imported for styling

const MainApp = () => {
  const location = useLocation();
  const [cardsData, setCardsData] = useState(initialCardsData);
  const [currentIndex, setCurrentIndex] = useState(0);
  const [bigIndex, setBigIndex] = useState(null); // Set to null initially
  const [showScrollDown, setShowScrollDown] = useState(true);
  const [showScrollUp, setShowScrollUp] = useState(false);
  const [loading, setLoading] = useState(true); // Added loading state
  const cardsContainerRef = useRef(null);

  const toggleSize = (index) => {
    setBigIndex(index === bigIndex ? null : index); // Handle null state
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

  useEffect(() => {
    // Simulate a delay to show the loader
    const timer = setTimeout(() => {
      setLoading(false);
    }, 2000); // 2 seconds delay

    return () => clearTimeout(timer);
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
      {loading ? ( // Show loader while loading is true
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

const App = () => (
  <Router>
    <MainApp />
  </Router>
);

export default App;
