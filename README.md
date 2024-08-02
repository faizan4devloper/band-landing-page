{
  "imageUrl": "SmartRecruit",
  "title": "Smart Recruit",
  "description": "A powerful tool for streamlining the recruitment process using AI and automation.",
  "industry": "HR",
  "businessFunction": "Recruitment",
  "content": {
    "description": [
      "smartRecruitDescription1",
      "smartRecruitDescription2"
    ],
    "solutionFlow": [
      "SmartRecruitFlow1",
      "SmartRecruitFlow2"
    ],
    "demo": "SmartRecruitDemo",
    "techArchitecture": "SmartRecruitArchitecture",
    "benefits": "smartRecruitBenefits",
    "adoption": "smartRecruitAdoption"
  }
}



const S3_BASE_URL = 'https://aiml-convai.s3.amazonaws.com/portal-slides';

const generateS3Url = (category, subcategory, filename) => 
  `${S3_BASE_URL}/${category}/${subcategory}/${filename}`;

export const dynamicAssets = {
  // CitizenAdvisor assets
  citizenDescription: generateS3Url('citizenadvisor', 'description', 'CitizenAdvisorDescription.png'),
  citizenBenefits: generateS3Url('citizenadvisor', 'benefits', 'CitizenAdvisorBenefits.png'),
  citizenAdoption: generateS3Url('citizenadvisor', 'industry-adoption', 'CitizenAdvisorAdoption.png'),
  CitizenAdvisorFlow1: generateS3Url('citizenadvisor', 'solutionFlows', 'CitizenAdvisorFlow1.png'),
  CitizenAdvisorFlow2: generateS3Url('citizenadvisor', 'solutionFlows', 'CitizenAdvisorFlow2.png'),
  CitizenAdvisorFlow3: generateS3Url('citizenadvisor', 'solutionFlows', 'CitizenAdvisorFlow3.png'),
  CitizenAdvisorFlow4: generateS3Url('citizenadvisor', 'solutionFlows', 'CitizenAdvisorFlow4.png'),
  CitizenAdvisorFlow5: generateS3Url('citizenadvisor', 'solutionFlows', 'CitizenAdvisorFlow5.png'),
  CitizenAdvisorArchitecture: generateS3Url('citizenadvisor', 'technical-architecture', 'CitizenAdvisorArchitecture.png'),

  // SmartRecruit assets
  smartRecruitDescription1: generateS3Url('smart-recruit', 'description', 'SmartRecruitDescription1.png'),
  smartRecruitDescription2: generateS3Url('smart-recruit', 'description', 'SmartRecruitDescription2.png'),
  SmartRecruitFlow1: generateS3Url('smart-recruit', 'solutionFlows', 'SmartRecruitFlow1.png'),
  SmartRecruitFlow2: generateS3Url('smart-recruit', 'solutionFlows', 'SmartRecruitFlow2.png'),
  SmartRecruitDemo: generateS3Url('smart-recruit', 'demo', 'SmartRecruitDemo.mp4'),
  SmartRecruitArchitecture: generateS3Url('smart-recruit', 'technical-architecture', 'SmartRecruitArchitecture.png'),
  smartRecruitBenefits: generateS3Url('smart-recruit', 'benefits', 'SmartRecruitBenefits.png'),
  smartRecruitAdoption: generateS3Url('smart-recruit', 'industry-adoption', 'SmartRecruitAdoption.png'),
};

export const images = {
  IntelligentAssist: require('./components/Cards/CardsImages/card3.jpg'),
  EmailEAR: require('./components/Cards/CardsImages/card1.jpg'),
  CaseIntelligence: require('./components/Cards/CardsImages/card4.jpg'),
  SmartRecruit: require('./components/Cards/CardsImages/SmartRecruit.jpg'),
  // Add other images as needed...
};

export const videos = {
  EmailEARDemo: 'https://aiml-convai.s3.amazonaws.com/demovideos/Email-EAR_Demo_new.mp4',
  SmartRecruitDemo: dynamicAssets.SmartRecruitDemo,
  // Add other videos as needed...
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

import { dynamicAssets } from './AssetImports';

function mapAssets(card) {
  const getDynamicAsset = (key) => {
    const asset = dynamicAssets[key];
    if (!asset) {
      console.warn(`Asset not found for key: ${key}`);
    }
    return asset || null;
  };

  const getArray = (data) => Array.isArray(data) ? data : [data];

  return {
    ...card,
    imageUrl: getDynamicAsset(card.imageUrl),
    content: {
      ...card.content,
      solutionFlow: getArray(card.content.solutionFlow).map(getDynamicAsset),
      demo: [getDynamicAsset(card.content.demo)],
      techArchitecture: getArray(card.content.techArchitecture).map(getDynamicAsset),
      description: getArray(card.content.description).map(getDynamicAsset),
      benefits: getArray(card.content.benefits).map(getDynamicAsset),
      adoption: getArray(card.content.adoption).map(getDynamicAsset),
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
