Uncaught TypeError: card.content.adoption.split is not a function
    at mapAssets (data.js:37:1)
    at ./src/data.js (data.js:43:1)
    at options.factory (react refresh:6:1)
    at __webpack_require__ (bootstrap:22:1)
    at fn (hot module replacement:61:1)
    at ./src/components/Sidebar/SideBarPage.js (SideBar.js:51:1)
    at options.factory (react refresh:6:1)
    at __webpack_require__ (bootstrap:22:1)
    at fn (hot module replacement:61:1)
    at ./src/App.js (SolutionFlow.svg:31:1)


    
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
      adoptionFlow: card.content.adoption ? adoption[card.content.adoption.split('.').pop()] : null, // Corrected here
    },
  };
}
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
// Import other card JSON files similarly

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
      adoptionFlow: card.content.adoption ? adoption[card.content.adoptions.split('.').pop()] : null,
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
  // Map other card JSON files similarly
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

  const toggleMaximize = (imageSrc) => {
    setMaximizedImage(maximizedImage === imageSrc ? null : imageSrc);
  };

  if (!content || !content.description) {
    return <div className={styles.mainContent}>Description not available</div>;
  }


 
  const contentMap = {
    description: (
      <div className={styles.description}>
        <h2>Description</h2>
         <img
          src={content.descriptionFlow}
          alt="description Flow"
          className={maximizedImage === content.descriptionFlow ? styles.maximized : ""}
          onClick={() => toggleMaximize(content.descriptionFlow)}
        />
      </div>
    ),
    solutionFlow: (
      <div className={styles.solution}>
        <h2>Solution Flow</h2>
        <Carousel
        showArrows={false}
        // showThumbs={false}
        showIndicators={false}
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
    ),
    demo: (
      <div className={styles.demo}>
        <h2>Demo</h2>
        <Video src={content.demo} />
      </div>
    ),
    techArchitecture: (
      <div className={styles.architecture}>
        <h2>Technical Architecture</h2>
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
        <h2>Benefits</h2>
        <img
          src={content.benefitsFlow}
          alt="benefits Flow"
          className={maximizedImage === content.benefitsFlow ? styles.maximized : ""}
          onClick={() => toggleMaximize(content.benefitsFlow)}
        />
      </div>
    ),
    adoptions: (
      <div className={styles.adoption}>
        <h2>Solution Adoption</h2>
        <img
          src={content.adoptionFlow}
          alt="adoption Flow"
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
          <FontAwesomeIcon icon={faTimes} className={styles.closeIcon} onClick={()=> setMaximizedImage(null)}/>
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
    "description": "descriptions.DescriptionDemo",
    "solutionFlow": ["solutionFlows.CitizenAdvisorFlow1", "solutionFlows.CitizenAdvisorFlow2", "solutionFlows.CitizenAdvisorFlow3", "solutionFlows.CitizenAdvisorFlow4"],
    "demo": "videos.demoVideo5",
    "techArchitecture": "architectures.CitizenAdvisorArchitecture",
    "benefits": "solutionsBenefits.citizenBenefits",
    "adoption": "adoption.citizenAdoption"
  }
}
