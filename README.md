// AssetImports.js

export const images = {
  IntelligentAssist: require('./components/Cards/CardsImages/card3.jpg'),
  EmailEAR: require('./components/Cards/CardsImages/card1.jpg'),
  CaseIntelligence: require('./components/Cards/CardsImages/card4.jpg'),
  SmartRecruit: require('./components/Cards/CardsImages/card8.jpg'),
  IAssureClaim: require('./components/Cards/CardsImages/card9.jpg'),
  AssistantEV: require('./components/Cards/CardsImages/card10.jpg'),
  AutoWiseCompanion: require('./components/Cards/CardsImages/card19.jpg'),
  CitizenAdvisor: require('./components/Cards/CardsImages/dummy2.jpg'),
  FinCompetitor: require('./components/Cards/CardsImages/card82.jpg'),
  SignatureExtraction: require('./components/Cards/CardsImages/card2.jpg'),
  AiForce: require('./components/Cards/CardsImages/card19.jpg'),
  ApiCase: require('./components/Cards/CardsImages/card13.jpg'),
  AmsSupport: require('./components/Cards/CardsImages/AUTOMATION.jpg'),
  CodeGreat: require('./components/Cards/CardsImages/card5.jpg'),
  AaigApi: require('./components/Cards/CardsImages/card16.jpg'),
  ResponsibleGen: require('./components/Cards/CardsImages/card17.jpg'),
  GraphData: require('./components/Cards/CardsImages/card18.jpg'),
  PredictiveAsset: require('./components/Cards/CardsImages/card11.jpg'),
};

export const videos = {
  EmailEARDemo: 'https://aiml-convai.s3.amazonaws.com/demovideos/Email-EAR_Demo_new.mp4',
  SignatureExtractionDemo: 'https://aiml-convai.s3.amazonaws.com/demovideos/Sign_Verification_New.mp4',
  IntelligentAssistDemo: 'https://aiml-convai.s3.amazonaws.com/demovideos/Intelligent_Assist-QnA_DemoVideo_new.mp4',
  CaseIntelligenceDemo: 'https://aiml-convai.s3.amazonaws.com/demovideos/Case-Intelligence_demo.mp4',
  CodeGReatDemo: 'https://aiml-convai.s3.amazonaws.com/demovideos/CodeGreat_Demo_new.mp4',
  SmartRecruitDemo: 'https://aiml-convai.s3.amazonaws.com/demovideos/SmartRecruit_IvAssist_Demo.mp4',
};

export const solutionFlows = {
  EmailEarFlow: require('./components/Sidebar/Icons/EmailEarFlowGraph.png'),
  IntelligentAssistFlow: require('./components/Sidebar/Icons/IntelligentAssistFlowGraph.png'),
  CaseIntelligenceFlow: require('./components/Sidebar/Icons/CaseIntelligenceFlowGraph.png'),
  SmartRecruitFlow: require('./components/Sidebar/Icons/SmartRecruitFlowGraph.png'),
  IAssureClaimFlow: require('./components/Sidebar/Icons/IAssureClaimFlowGraph.png'),
};

export const architectures = {
  IntelligentAssistArchitecture: require('./components/Sidebar/Icons/IntelligentAssistarchitecture.png'),
  EmailEARArchitecture: require('./components/Sidebar/Icons/EmailEARarchitecture.png'),
  CaseIntelligenceArchitecture: require('./components/Sidebar/Icons/CaseIntelligencearchitecture.png'),
  SmartRecruitArchitecture: require('./components/Sidebar/Icons/SmartRecruitarchitecture.png'),
  AssistantEvArchitecture: require('./components/Sidebar/Icons/AssistantEvachitecture.png'),
  IAssureClaimArchitecture: require('./components/Sidebar/Icons/IAssureClaimarchitecture.png'),
  AIForceArchitecture: require('./components/Sidebar/Icons/AIForcearchitecture.png'),
};

export const descriptions = {
  DescriptionDemo: require('./components/Sidebar/Icons/CitizenAdvisorDesc.png'),
};


{
  "imageUrl": "images.IntelligentAssist",
  "title": "Intelligent Assist",
  "description": "Intelligent Assist provides an experience transformation...",
  "industry": "Technology",
  "businessFunction": "Customer Support",
  "content": {
    "descriptionFlow": "descriptions.DescriptionDemo",
    "solutionFlow": "solutionFlows.IntelligentAssistFlow",
    "demo": "videos.IntelligentAssistDemo",
    "techArchitecture": "architectures.IntelligentAssistArchitecture",
    "benefits": "By bringing together key capabilities for developers...",
    "adoption": [
      {"industry": "Financial", "adoption": "The solution could explain complex financial products..."},
      {"industry": "Education", "adoption": "Can serve as virtual tutors..."},
      // Add other industry adoption examples here...
    ]
  }
}


// MainContent.js

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

import { images, videos, solutionFlows, architectures, descriptions } from './AssetImports';

function mapAssets(card) {
  return {
    ...card,
    imageUrl: card.imageUrl ? images[card.imageUrl.split('.').pop()] : null,
    content: {
      ...card.content,
      solutionFlow: card.content.solutionFlow ? solutionFlows[card.content.solutionFlow.split('.').pop()] : null,
      demo: card.content.demo ? videos[card.content.demo.split('.').pop()] : null,
      techArchitecture: card.content.techArchitecture ? architectures[card.content.techArchitecture.split('.').pop()] : null,
      descriptionFlow: card.content.descriptionFlow ? descriptions[card.content.descriptionFlow.split('.').pop()] : null,
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

  const keywords = [
    "extract", "Act", "Respond", "query", "complaint", "issue", "generates", "user-friendly", "questions", "concerns", "detailed response", "prioritization", "queuing", "delayed responses", "Gen AI-powered", "automating", "reading", "analysis", "thoughtful responding", "customer experience", "automates", "Gen AI-powered", "solution", "organization", "intelligent", "assist", "data capture", "manual processes", "Email EAR", "(Extract, Act and Respond)", "Unified experience"
  ];

  const highlightKeywords = (text) => {
    const regex = new RegExp(`\\b(${keywords.join("|")})\\b`, "gi");
    return text.replace(regex, (matched) => `<span class="${styles.highlight}">${matched}</span>`);
  };

  const descriptionPoints = content.description.split(". ").map((point, index) => (
    <li key={index} dangerouslySetInnerHTML={{ __html: highlightKeywords(point.trim()) }}></li>
  ));

  const benefitsPoints = content.benefits.split(". ").map((point, index) => (
    <li key={index} dangerouslySetInnerHTML={{ __html: highlightKeywords(point.trim()) }}></li>
  ));

  const adoptionRows = content.adoption.map((row, index) => (
    <tr key={index}>
      <td>{row.industry}</td>
      <td>{row.adoption}</td>
    </tr>
  ));

  return (
    <div className={styles.mainContent}>
      <h2>{activeTab}</h2>
      <div className={styles.contentSection}>
        <h3>Description</h3>
        <ul>{descriptionPoints}</ul>
      </div>
      <div className={styles.contentSection}>
        <h3>Benefits</h3>
        <ul>{benefitsPoints}</ul>
      </div>
      {content.demo && <Video videoUrl={content.demo} />}
      <div className={styles.contentSection}>
        <h3>Adoption</h3>
        <table className={styles.adoptionTable}>
          <thead>
            <tr>
              <th>Industry</th>
              <th>Adoption</th>
            </tr>
          </thead>
          <tbody>{adoptionRows}</tbody>
        </table>
      </div>
      {content.solutionFlow && (
        <div className={styles.imageContainer}>
          <img
            src={content.solutionFlow}
            alt="Solution Flow"
            className={`${styles.image} ${maximizedImage === content.solutionFlow ? styles.maximized : ""}`}
            onClick={() => toggleMaximize(content.solutionFlow)}
          />
          {maximizedImage === content.solutionFlow && (
            <FontAwesomeIcon icon={faTimes} className={styles.closeIcon} onClick={() => toggleMaximize(null)} />
          )}
        </div>
      )}
      {content.descriptionFlow && (
        <div className={styles.imageContainer}>
          <img
            src={content.descriptionFlow}
            alt="Description Flow"
            className={`${styles.image} ${maximizedImage === content.descriptionFlow ? styles.maximized : ""}`}
            onClick={() => toggleMaximize(content.descriptionFlow)}
          />
          {maximizedImage === content.descriptionFlow && (
            <FontAwesomeIcon icon={faTimes} className={styles.closeIcon} onClick={() => toggleMaximize(null)} />
          )}
        </div>
      )}
      {content.techArchitecture && (
        <div className={styles.imageContainer}>
          <img
            src={content.techArchitecture}
            alt="Technical Architecture"
            className={`${styles.image} ${maximizedImage === content.techArchitecture ? styles.maximized : ""}`}
            onClick={() => toggleMaximize(content.techArchitecture)}
          />
          {maximizedImage === content.techArchitecture && (
            <FontAwesomeIcon icon={faTimes} className={styles.closeIcon} onClick={() => toggleMaximize(null)} />
          )}
        </div>
      )}
    </div>
  );
};

export default MainContent;
