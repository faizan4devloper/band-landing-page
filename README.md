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
  // Static imports
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
  // Dynamically loaded images
  ...assetUrls.images
};

export const videos = {
  // Static videos
  EmailEARDemo: 'https://aiml-convai.s3.amazonaws.com/demovideos/Email-EAR_Demo_new.mp4',
  SignatureExtractionDemo: 'https://aiml-convai.s3.amazonaws.com/demovideos/Sign_Verification_New.mp4',
  IntelligentAssistDemo: 'https://aiml-convai.s3.amazonaws.com/demovideos/Intelligent_Assist-QnA_DemoVideo_new.mp4',
  CaseIntelligenceDemo: 'https://aiml-convai.s3.amazonaws.com/demovideos/Case-Intelligence_demo.mp4',
  CodeGReatDemo: 'https://aiml-convai.s3.amazonaws.com/demovideos/CodeGreat_Demo_new.mp4',
  SmartRecruitDemo: 'https://aiml-convai.s3.amazonaws.com/demovideos/SmartRecruit_IvAssist_Demo.mp4',
  CitizenAdvisorDemo: 'https://aiml-convai.s3.amazonaws.com/demovideos/Citizen_Advisor-Demo1.mp4',
  // Dynamically loaded videos
  ...assetUrls.videos
};

export const architectures = {
  // Dynamically loaded architectures
  ...assetUrls.architectures
};

export const solutionFlows = {
  // Dynamically loaded solution flows
  ...assetUrls.solutionFlows
};

export const descriptions = {
  // Dynamically loaded descriptions
  ...assetUrls.descriptions
};

export const solutionsBenefits = {
  // Dynamically loaded solutions benefits
  ...assetUrls.solutionsBenefits
};

export const adoption = {
  // Dynamically loaded adoption
  ...assetUrls.adoption
};
