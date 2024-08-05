// AssetImports.js
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

export const videos = {
  EmailEARDemo: 'https://aiml-convai.s3.amazonaws.com/demovideos/Email-EAR_Demo_new.mp4',
  SignatureExtractionDemo: 'https://aiml-convai.s3.amazonaws.com/demovideos/Sign_Verification_New.mp4',
  IntelligentAssistDemo: 'https://aiml-convai.s3.amazonaws.com/demovideos/Intelligent_Assist-QnA_DemoVideo_new.mp4',
  CaseIntelligenceDemo: 'https://aiml-convai.s3.amazonaws.com/demovideos/Case-Intelligence_demo.mp4',
  CodeGReatDemo: 'https://aiml-convai.s3.amazonaws.com/demovideos/CodeGreat_Demo_new.mp4',
  SmartRecruitDemo: 'https://aiml-convai.s3.amazonaws.com/demovideos/SmartRecruit_IvAssist_Demo.mp4',
  CitizenAdvisorDemo: 'https://aiml-convai.s3.amazonaws.com/demovideos/Citizen_Advisor-Demo1.mp4',
};

// Solution Flows, Technical Architectures, Descriptions, Solutions Benefits, and Adoption will be handled dynamically
export const solutionFlows = {};
export const architectures = {};
export const descriptions = {};
export const solutionsBenefits = {};
export const adoption = {};




import axios from 'axios';
import { images, videos } from './AssetImports';

const URL_JSON_URL = 'https://aiml-convai.s3.amazonaws.com/URLJson.json';

async function fetchURLJson() {
  const response = await axios.get(URL_JSON_URL);
  return response.data;
}

function mapAssets(card, assets) {
  return {
    ...card,
    imageUrl: card.imageUrl ? assets.images[card.imageUrl] : null,
    content: {
      ...card.content,
      solutionFlow: Array.isArray(card.content.solutionFlow)
        ? card.content.solutionFlow.map(flow => assets.solutionFlows[flow])
        : [],
      demo: typeof card.content.demo === 'string'
        ? assets.videos[card.content.demo]
        : null,
      techArchitecture: Array.isArray(card.content.techArchitecture)
        ? card.content.techArchitecture.map(arch => assets.architectures[arch])
        : [],
      descriptionFlow: Array.isArray(card.content.description)
        ? card.content.description.map(desc => assets.descriptions[desc])
        : [],
      benefitsFlow: Array.isArray(card.content.benefits)
        ? card.content.benefits.map(benefit => assets.solutionsBenefits[benefit])
        : [],
      adoptionFlow: Array.isArray(card.content.adoption)
        ? card.content.adoption.map(adopt => assets.adoption[adopt])
        : [],
    },
  };
}

async function getCardsData() {
  const urlJson = await fetchURLJson();

  const assets = {
    images: images,  // Static images from AssetImports
    videos: videos,  // Static videos from AssetImports
    solutionFlows: urlJson.solutionFlows || {},
    architectures: urlJson.architectures || {},
    descriptions: urlJson.descriptions || {},
    solutionsBenefits: urlJson.solutionsBenefits || {},
    adoption: urlJson.adoption || {},
  };

  const cardDataFiles = [
    'IntelligentAssist',
    'EmailEAR',
    'CaseIntelligence',
    'SmartRecruit',
    'IAssureClaim',
    'AssistantEV',
    'AutoWiseCompanion',
    'CitizenAdvisor',
    'FinCompetitor',
    'SignatureExtraction',
    'AiForce',
    'ApiCase',
    'AmsSupport',
    'CodeGreat',
    'AaigApi',
    'ResponsibleGen',
    'GraphData',
    'PredictiveAsset'
  ];

  const cardDataPromises = cardDataFiles.map(file =>
    import(`./CardsData/${file}.json`).then(module => mapAssets(module.default, assets))
  );

  const cardsData = await Promise.all(cardDataPromises);
  return cardsData;
}

export default getCardsData;
