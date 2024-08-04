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





// data.js

import { images, videos, fetchDynamicAssets, getDynamicAssetUrl } from './AssetImports';

// Static card data
import IntelligentAssist from './CardsData/IntelligentAssist.json';
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
    // Map other cards similarly...
  ];
}

// Get cards data
export const cardsData = getDynamicCardsData();
