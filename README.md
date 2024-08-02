// utils/fetchAssetUrls.js
export const fetchAssetUrls = async () => {
  try {
    const response = await fetch('https://aiml-convai.s3.amazonaws.com/portal-slides/urldata.json');
    const data = await response.json();

    const assetUrls = {
      images: {},
      videos: {},
      architectures: {},
      solutionFlows: {},
      descriptions: {},
      solutionsBenefits: {},
      adoption: {}
    };

    data.forEach(item => {
      const key = Object.keys(item)[0];
      const value = item[key];

      if (key.includes('benefits')) {
        assetUrls.solutionsBenefits[key] = value;
      } else if (key.includes('techArchitecture')) {
        assetUrls.architectures[key] = value;
      } else if (key.includes('solutionFlow')) {
        assetUrls.solutionFlows[key] = value;
      } else if (key.includes('description')) {
        assetUrls.descriptions[key] = value;
      } else if (key.includes('adoption')) {
        assetUrls.adoption[key] = value;
      }
    });

    return assetUrls;
  } catch (error) {
    console.error('Error fetching asset URLs:', error);
    return {};
  }
};


// AssetImport.js
import { fetchAssetUrls } from './fetchAssetUrls';

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
  // Static imports (if needed)
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
  ...assetUrls.images // Dynamically loaded images
};

export const videos = {
  // Static videos (if needed)
  EmailEARDemo: 'https://aiml-convai.s3.amazonaws.com/demovideos/Email-EAR_Demo_new.mp4',
  SignatureExtractionDemo: 'https://aiml-convai.s3.amazonaws.com/demovideos/Sign_Verification_New.mp4',
  IntelligentAssistDemo: 'https://aiml-convai.s3.amazonaws.com/demovideos/Intelligent_Assist-QnA_DemoVideo_new.mp4',
  CaseIntelligenceDemo: 'https://aiml-convai.s3.amazonaws.com/demovideos/Case-Intelligence_demo.mp4',
  CodeGReatDemo: 'https://aiml-convai.s3.amazonaws.com/demovideos/CodeGreat_Demo_new.mp4',
  SmartRecruitDemo: 'https://aiml-convai.s3.amazonaws.com/demovideos/SmartRecruit_IvAssist_Demo.mp4',
  CitizenAdvisorDemo: 'https://aiml-convai.s3.amazonaws.com/demovideos/Citizen_Advisor-Demo1.mp4',
  ...assetUrls.videos // Dynamically loaded videos
};

export const architectures = {
  ...assetUrls.architectures // Dynamically loaded architectures
};

export const solutionFlows = {
  ...assetUrls.solutionFlows // Dynamically loaded solution flows
};

export const descriptions = {
  ...assetUrls.descriptions // Dynamically loaded descriptions
};

export const solutionsBenefits = {
  ...assetUrls.solutionsBenefits // Dynamically loaded solutions benefits
};

export const adoption = {
  ...assetUrls.adoption // Dynamically loaded adoption
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
        ? card.content.solutionFlow.map(flow => typeof flow === 'string' ? solutionFlows[flow.split('.').pop()] : null)
        : (typeof card.content.solutionFlow === 'string' ? [solutionFlows[card.content.solutionFlow.split('.').pop()]] : []),
      demo: card.content.demo ? videos[card.content.demo.split('.').pop()] : null,
      techArchitecture: Array.isArray(card.content.techArchitecture)
        ? card.content.techArchitecture.map(arch => typeof arch === 'string' ? architectures[arch.split('.').pop()] : null)
        : (typeof card.content.techArchitecture === 'string' ? [architectures[card.content.techArchitecture.split('.').pop()]] : []),
      descriptionFlow: Array.isArray(card.content.description)
        ? card.content.description.map(desc => typeof desc === 'string' ? descriptions[desc.split('.').pop()] : null)
        : (typeof card.content.description === 'string' ? [descriptions[card.content.description.split('.').pop()]] : []),
      benefitsFlow: Array.isArray(card.content.benefits)
        ? card.content.benefits.map(benefit => typeof benefit === 'string' ? solutionsBenefits[benefit.split('.').pop()] : null)
        : (typeof card.content.benefits === 'string' ? [solutionsBenefits[card.content.benefits.split('.').pop()]] : []),
      adoptionFlow: Array.isArray(card.content.adoption)
        ? card.content.adoption.map(adopt => typeof adopt === 'string' ? adoption[adopt.split('.').pop()] : null)
        : (typeof card.content.adoption === 'string' ? [adoption[card.content.adoption.split('.').pop()]] : []),
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
