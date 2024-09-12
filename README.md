import axios from 'axios';

// Define images object with dynamic imports for lazy loading
export const images = {
  IntelligentAssist: () => import('./components/Cards/CardsImages/IntelligentAssist.JPEG'),
  EmailEAR: () => import('./components/Cards/CardsImages/EmailEAR.JPEG'),
  CaseIntelligence: () => import('./components/Cards/CardsImages/CaseIntelligence.JPEG'),
  SmartRecruit: () => import('./components/Cards/CardsImages/SmartRecruit.JPEG'),
  ClaimAssist: () => import('./components/Cards/CardsImages/ClaimAssist.JPEG'),
  AssistantEV: () => import('./components/Cards/CardsImages/AssistantEV.JPEG'),
  CitizenAdvisor: () => import('./components/Cards/CardsImages/citizenAdvisor.JPEG'),
  FinanceCompetitor: () => import('./components/Cards/CardsImages/FinanceCompetitor.JPEG'),
  Signature: () => import('./components/Cards/CardsImages/Signature.JPEG'),
  AIForce: () => import('./components/Cards/CardsImages/AIForce.JPEG'),
  APICase: () => import('./components/Cards/CardsImages/APICase.JPEG'),
  AMSSupport: () => import('./components/Cards/CardsImages/AMSSupport.JPEG'),
  SOP: () => import('./components/Cards/CardsImages/SOP.JPEG'),
  CodeGReat: () => import('./components/Cards/CardsImages/CodeGReat.JPEG'),
  AAIG: () => import('./components/Cards/CardsImages/AAIG.JPEG'),
  ResponsibleGen: () => import('./components/Cards/CardsImages/ResponsibleGen.JPEG'),
  GraphData: () => import('./components/Cards/CardsImages/GraphData.JPEG'),
  PredictiveAsset: () => import('./components/Cards/CardsImages/PredictiveAsset.JPEG'),
  AutoWiseCampanian: () => import('./components/Cards/CardsImages/autowiseCompanian.JPEG'),
};

// Function to fetch assets from S3 or your API
export async function fetchAssets() {
  try {
    const response = await axios.get('xyz', {
      headers: {
        'Cache-Control': 'no-cache',
      }
    });

    console.log(response.data);
    return response.data;
  } catch (error) {
    console.error('Failed to fetch assets:', error);
    return {}; // Fallback to empty object if there is an error
  }
}



import IntelligentAssist from './CardsData/IntelligentAssist.json';
import EmailEAR from './CardsData/EmailEAR.json';
import CaseIntelligence from './CardsData/CaseIntelligence.json';
import SmartRecruit from './CardsData/SmartRecruit.json';
import ClaimAssist from './CardsData/ClaimAssist.json';
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
import SopAssistance from './CardsData/SopAssistance.json';

import { images, fetchAssets } from './AssetImports';

// Helper function to dynamically import images
async function getImage(imageKey) {
  try {
    const imageModule = await images[imageKey]();
    return imageModule.default;
  } catch (error) {
    console.error('Image load failed:', error);
    return 'defaultImagePath.jpg';
  }
}

// Function to map and fetch assets for a specific card
async function mapAssets(card) {
  const assets = await fetchAssets();
  
  // Sanitize title to match keys in your data
  const assetKey = card.title.replace(/\s+/g, '');
  
  const data = {
    solutionFlow: assets[`${assetKey}solutionFlow`] || null,
    techArchitecture: assets[`${assetKey}techArchitecture`] || null,
    description: assets[`${assetKey}description`] || null,
    benefits: assets[`${assetKey}benefits`] || null,
    adoption: assets[`${assetKey}adoption`] || null,
    demo: assets[`${assetKey}demo`] || assets[`${assetKey}Demo`] || null,
  };

  // Fetch the image dynamically
  const imageUrl = await getImage(card.imageUrl);

  return {
    ...card,
    imageUrl,
    content: {
      ...card.content,
      solutionFlow: data.solutionFlow,
      demo: data.demo || null,
      techArchitecture: data.techArchitecture,
      description: data.description,
      benefits: data.benefits,
      adoption: data.adoption,
    },
  };
}

// Function to fetch and map all cards data asynchronously
export async function getCardsData() {
  const cardsData = await Promise.all([
    mapAssets(IntelligentAssist),
    mapAssets(EmailEAR),
    mapAssets(CaseIntelligence),
    mapAssets(SmartRecruit),
    mapAssets(ClaimAssist),
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
    mapAssets(SopAssistance),
  ]);

  return cardsData;
}
















// AssetImports.js
import axios from 'axios';

export const images = {
  IntelligentAssist: require('./components/Cards/CardsImages/IntelligentAssist.JPEG'),
  EmailEAR: require('./components/Cards/CardsImages/EmailEAR.JPEG'),
  CaseIntelligence: require('./components/Cards/CardsImages/CaseIntelligence.JPEG'),
  SmartRecruit: require('./components/Cards/CardsImages/SmartRecruit.JPEG'),
  ClaimAssist: require('./components/Cards/CardsImages/ClaimAssist.JPEG'),
  AssistantEV: require('./components/Cards/CardsImages/AssistantEV.JPEG'),
  CitizenAdvisor: require('./components/Cards/CardsImages/citizenAdvisor.JPEG'),
  FinanceCompetitor: require('./components/Cards/CardsImages/FinanceCompetitor.JPEG'),
  Signature: require('./components/Cards/CardsImages/Signature.JPEG'),
  AIForce: require('./components/Cards/CardsImages/AIForce.JPEG'),
  APICase: require('./components/Cards/CardsImages/APICase.JPEG'),
  AMSSupport: require('./components/Cards/CardsImages/AMSSupport.JPEG'),
  SOP: require('./components/Cards/CardsImages/SOP.JPEG'),
  CodeGReat: require('./components/Cards/CardsImages/CodeGReat.JPEG'),
  AAIG: require('./components/Cards/CardsImages/AAIG.JPEG'),
  ResponsibleGen: require('./components/Cards/CardsImages/ResponsibleGen.JPEG'),
  GraphData: require('./components/Cards/CardsImages/GraphData.JPEG'),
  PredictiveAsset: require('./components/Cards/CardsImages/PredictiveAsset.JPEG'),
  AutoWiseCampanian: require('./components/Cards/CardsImages/autowiseCompanian.JPEG'),
};



// Fetch and cache JSON data using Axios with cache-busting
export async function fetchAssets() {
  try {
    const response = await axios.get(`xyz`, {
      headers: {
        'Cache-Control': 'no-cache', // Disable caching
      }
    });

    console.log(response.data);
    return response.data;
  } catch (error) {
    console.error('Failed to fetch assets:', error);
    return {}; // Fallback to an empty object in case of error
  }
}

fetchAssets().then(data => {
  console.log('Fetched Assets:', data);
});



import IntelligentAssist from './CardsData/IntelligentAssist.json';
import EmailEAR from './CardsData/EmailEAR.json';
import CaseIntelligence from './CardsData/CaseIntelligence.json';
import SmartRecruit from './CardsData/SmartRecruit.json';
import ClaimAssist from './CardsData/ClaimAssist.json';
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
import SopAssistance from './CardsData/SopAssistance.json';

import { images, fetchAssets } from './AssetImports';

// Mapping assets for a specific card
async function mapAssets(card) {
  const assets = await fetchAssets();
  
  // Sanitize the title to match the keys in urldata.json
  const assetKey = card.title.replace(/\s+/g, '');

  // Access the data for the specific asset
  const data = {
    solutionFlow: assets[`${assetKey}solutionFlow`] || null,
    techArchitecture: assets[`${assetKey}techArchitecture`] || null,
    description: assets[`${assetKey}description`] || null,
    benefits: assets[`${assetKey}benefits`] || null,
    adoption: assets[`${assetKey}adoption`] || null,
    demo: assets[`${assetKey}demo`] || assets[`${assetKey}Demo`] || null,
  };
  
  console.log(`Data for ${card.title}:`, data);

  return {
    ...card,
    imageUrl: images[card.imageUrl] || 'defaultImagePath.jpg', // Use a default image if the specified one is missing
    content: {
      ...card.content,
      solutionFlow: data.solutionFlow,
      demo: data.demo || null,
      techArchitecture: data.techArchitecture,
      description: data.description,
      benefits: data.benefits,
      adoption: data.adoption,
    },
  };
}

// Mapping cards data asynchronously
export async function getCardsData() {
  const cardsData = await Promise.all([
    mapAssets(IntelligentAssist),
    mapAssets(EmailEAR),
    mapAssets(CaseIntelligence),
    mapAssets(SmartRecruit),
    mapAssets(ClaimAssist),
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
    mapAssets(SopAssistance),
  ]);

  return cardsData;
}
