// utils/fetchAssetUrls.js
export const fetchAssetUrls = async () => {
  const response = await fetch('/path/to/urldata.json'); // Adjust the path as necessary
  if (!response.ok) {
    throw new Error('Failed to fetch asset URLs');
  }
  const data = await response.json();
  return data;
};



// AssetImport.js
import { fetchAssetUrls } from './utils/fetchAssetUrls';

let assetUrls = {};

const loadAssets = async () => {
  try {
    assetUrls = await fetchAssetUrls();
  } catch (error) {
    console.error('Error loading asset URLs:', error);
  }
};

loadAssets(); // Load the assets when the module is imported

export const images = {
  // Keep existing static imports
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

// Technical Architectures
export const architectures = {};

// Solution Flows
export const solutionFlows = {};

// Descriptions
export const descriptions = {};

// Solutions Benefits
export const solutionsBenefits = {};

// Adoption
export const adoption = {};




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

import { images, videos, solutionFlows, architectures, descriptions, solutionsBenefits, adoption } from './AssetImport';

function mapAssets(card) {
  return {
    ...card,
    imageUrl: images[card.imageUrl] || null,
    content: {
      ...card.content,
      solutionFlow: Array.isArray(card.content.solutionFlow)
        ? card.content.solutionFlow.map(flow => solutionFlows[flow] || null)
        : (typeof card.content.solutionFlow === 'string' ? [solutionFlows[card.content.solutionFlow] || null] : []),
      demo: videos[card.content.demo] || null,
      techArchitecture: Array.isArray(card.content.techArchitecture)
        ? card.content.techArchitecture.map(arch => architectures[arch] || null)
        : (typeof card.content.techArchitecture === 'string' ? [architectures[card.content.techArchitecture] || null] : []),
      descriptionFlow: Array.isArray(card.content.description)
        ? card.content.description.map(desc => descriptions[desc] || null)
        : (typeof card.content.description === 'string' ? [descriptions[card.content.description] || null] : []),
      benefitsFlow: Array.isArray(card.content.benefits)
        ? card.content.benefits.map(benefit => solutionsBenefits[benefit] || null)
        : (typeof card.content.benefits === 'string' ? [solutionsBenefits[card.content.benefits] || null] : []),
      adoptionFlow: Array.isArray(card.content.adoption)
        ? card.content.adoption.map(adopt => adoption[adopt] || null)
        : (typeof card.content.adoption === 'string' ? [adoption[card.content.adoption] || null] : []),
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

import { images, videos, solutionFlows, architectures, descriptions, solutionsBenefits, adoption } from './AssetImport';

function mapAssets(card) {
  return {
    ...card,
    imageUrl: images[card.imageUrl] || null,
    content: {
      ...card.content,
      solutionFlow: Array.isArray(card.content.solutionFlow)
        ? card.content.solutionFlow.map(flow => solutionFlows[flow] || null)
        : (typeof card.content.solutionFlow === 'string' ? [solutionFlows[card.content.solutionFlow] || null] : []),
      demo: videos[card.content.demo] || null,
      techArchitecture: Array.isArray(card.content.techArchitecture)
        ? card.content.techArchitecture.map(arch => architectures[arch] || null)
        : (typeof card.content.techArchitecture === 'string' ? [architectures[card.content.techArchitecture] || null] : []),
      descriptionFlow: Array.isArray(card.content.description)
        ? card.content.description.map(desc => descriptions[desc] || null)
        : (typeof card.content.description === 'string' ? [descriptions[card.content.description] || null] : []),
      benefitsFlow: Array.isArray(card.content.benefits)
        ? card.content.benefits.map(benefit => solutionsBenefits[benefit] || null)
        : (typeof card.content.benefits === 'string' ? [solutionsBenefits[card.content.benefits] || null] : []),
      adoptionFlow: Array.isArray(card.content.adoption)
        ? card.content.adoption.map(adopt => adoption[adopt] || null)
        : (typeof card.content.adoption === 'string' ? [adoption[card.content.adoption] || null] : []),
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
