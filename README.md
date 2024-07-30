
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
};

// Solution Flows
export const solutionFlows = {
  EmailEarFlow: require('./components/Sidebar/Icons/EmailEarFlowGraph.png'),
  IntelligentAssistFlow: require('./components/Sidebar/Icons/IntelligentAssistFlowGraph.png'),
  CaseIntelligenceFlow: require('./components/Sidebar/Icons/CaseIntelligenceFlowGraph.png'),
  SmartRecruitFlow: require('./components/Sidebar/Icons/SmartRecruitFlowGraph.png'),
  IAssureClaimFlow: require('./components/Sidebar/Icons/IAssureClaimFlowGraph.png'),
  CitizenAdvisorFlow1: require('./components/Sidebar/Icons/Slide1.png'),
  CitizenAdvisorFlow2: require('./components/Sidebar/Icons/Slide2.png'),
  CitizenAdvisorFlow3: require('./components/Sidebar/Icons/Slide3.png'),
  CitizenAdvisorFlow4: require('./components/Sidebar/Icons/Slide4.png'),
  
  SmartRecruitSolutionFlow1: require('./components/Sidebar/Icons/SmartRecruitSolutionFlow1.png'),
  SmartRecruitSolutionFlow2: require('./components/Sidebar/Icons/SmartRecruitSolutionFlow2.png'),
  
  
  
  
};

// Technical Architectures
export const architectures = {
  IntelligentAssistArchitecture: require('./components/Sidebar/Icons/IntelligentAssistarchitecture.png'),
  EmailEARArchitecture: require('./components/Sidebar/Icons/EmailEARarchitecture.png'),
  CaseIntelligenceArchitecture: require('./components/Sidebar/Icons/CaseIntelligencearchitecture.png'),
  SmartRecruitArchitecture: require('./components/Sidebar/Icons/SmartRecruitArchitecture.png'),
  AssistantEvArchitecture: require('./components/Sidebar/Icons/AssistantEvachitecture.png'),
  IAssureClaimArchitecture: require('./components/Sidebar/Icons/IAssureClaimarchitecture.png'),
  AIForceArchitecture: require('./components/Sidebar/Icons/AIForcearchitecture.png'),
  CitizenAdvisorArchitecture: require('./components/Sidebar/Icons/Slide7.png'),
  // DescriptionDemo: require('./components/Sidebar/Icons/CitizenAdvisorDesc.png'),
};


export const descriptions = {
  citizenDescription: require('./components/Sidebar/Icons/CitizenAdvisorDesc.png'),
  smartRecruitDescription: require('./components/Sidebar/Icons/SmartRecruitDescription1.png'),
};

export const solutionsBenefits = {
  citizenBenefits: require('./components/Sidebar/Icons/CitizenAdvisorBenefits.png'),
  smartRecruitBenefits: require('./components/Sidebar/Icons/SmartRecruitBenefits.png'),
};
export const adoption = {
  citizenAdoption: require('./components/Sidebar/Icons/CitizenAdoption.png'),
  smartRecruitAdoption: require('./components/Sidebar/Icons/SmartRecruitAdoption.png'),
};



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


const { images, videos, solutionFlows, architectures, descriptions, solutionsBenefits, adoption } = require('./AssetImports');

function mapAssets(card) {
  return {
    ...card,
    imageUrl: card.imageUrl ? images[card.imageUrl.split('.').pop()] : null,
    content: {
      ...card.content,
      solutionFlow: Array.isArray(card.content.solutionFlow) 
        ? card.content.solutionFlow.map(flow => solutionFlows[flow.split('.').pop()]) 
        : [],
      demo: card.content.demo ? videos[card.content.demo.split('.').pop()] : null,
      techArchitecture: card.content.techArchitecture ? architectures[card.content.techArchitecture.split('.').pop()] : null,
      descriptionFlow: card.content.description ? descriptions[card.content.description.split('.').pop()] : null,
      benefitsFlow: card.content.benefits ? solutionsBenefits[card.content.benefits.split('.').pop()] : null,
      adoptionFlow: typeof card.content.adoption === 'string' ? adoption[card.content.adoption.split('.').pop()] : null,
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

  if (!content || !content.description) {
    return <div className={styles.mainContent}>Description not available</div>;
  }

  const contentMap = {
    description: (
      <div className={styles.description}>
        <img
          src={content.descriptionFlow}
          alt="Description Flow"
          className={maximizedImage === content.descriptionFlow ? styles.maximized : ""}
          onClick={() => toggleMaximize(content.descriptionFlow)}
        />
      </div>
    ),
    solutionFlow: (
      <div className={styles.solution}>
        <div className={styles.carouselContainer}>
          <div className={styles.customThumbs}>
            {content.solutionFlow.map((image, index) => (
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
            {content.solutionFlow.map((image, index) => (
              <div key={index}>
                <img
                  src={image}
                  alt={`Solution Flow ${index + 1}`}
                  className={maximizedImage === image ? styles.maximized : ""}
                  onClick={() => toggleMaximize(image)}
                />
              </div>
            ))}
          </Carousel>
        </div>
      </div>
    ),
    demo: (
      <div className={styles.demo}>
        <Video src={content.demo} />
      </div>
    ),
    techArchitecture: (
      <div className={styles.architecture}>
        <img
          src={content.techArchitecture}
          alt="Technical Architecture"
          className={maximizedImage === content.techArchitecture ? styles.maximized : ""}
          onClick={() => toggleMaximize(content.techArchitecture)}
        />
      </div>
    ),
    benefits: (
      <div className={styles.benefits}>
        <img
          src={content.benefitsFlow}
          alt="Benefits Flow"
          className={maximizedImage === content.benefitsFlow ? styles.maximized : ""}
          onClick={() => toggleMaximize(content.benefitsFlow)}
        />
      </div>
    ),
    adoption: (
      <div className={styles.adoption}>
        <img
          src={content.adoptionFlow}
          alt="Adoption Flow"
          className={maximizedImage === content.adoptionFlow ? styles.maximized : ""}
          onClick={() => toggleMaximize(content.adoptionFlow)}
        />
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

{
  "imageUrl": "images.CitizenAdvisor",
  "title": "Citizen Advisor",
  "description": "An experience transformation from disconnected silos information to an intuitive, personalized revelations",
  "industry": "GOVT",
  "businessFunction": "Customer Experience",
  "content": {
    "description": "descriptions.citizenDescription",
    "solutionFlow": ["solutionFlows.CitizenAdvisorFlow1", "solutionFlows.CitizenAdvisorFlow2", "solutionFlows.CitizenAdvisorFlow3", "solutionFlows.CitizenAdvisorFlow4"],
    "demo": "videos.demoVideo5",
    "techArchitecture": "architectures.CitizenAdvisorArchitecture",
    "benefits": "solutionsBenefits.citizenBenefits",
    "adoption": "adoption.citizenAdoption"
  }
}
