{
  "descriptions": {
    "citizenDescription": "https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisor/description/CitizenAdvisorDescription.png",
    "smartRecruitDescription1": "https://aiml-convai.s3.amazonaws.com/portal-slides/smart-recruit/description/SmartRecruitDescription1.png",
    "smartRecruitDescription2": "https://aiml-convai.s3.amazonaws.com/portal-slides/smart-recruit/description/SmartRecruitDescription2.png"
  },
  "solutionFlows": {
    "baseURL": "https://aiml-convai.s3.amazonaws.com/portal-slides",
    "paths": {
      "CitizenAdvisor": "citizenadvisor/solutionFlows/CitizenAdvisorFlow",
      "SmartRecruit": "smart-recruit/solutionFlows/SmartRecruitSolutionFlow"
    },
    "count": 5
  },
  "architectures": {
    "IntelligentAssistArchitecture": "./components/Sidebar/Icons/IntelligentAssistarchitecture.png",
    "EmailEARArchitecture": "./components/Sidebar/Icons/EmailEARarchitecture.png",
    "CaseIntelligenceArchitecture": "./components/Sidebar/Icons/CaseIntelligencearchitecture.png",
    "SmartRecruitArchitecture": "https://aiml-convai.s3.amazonaws.com/portal-slides/smart-recruit/technical-architecture/SmartRecruitArchitecture.png",
    "AssistantEvArchitecture": "./components/Sidebar/Icons/AssistantEvachitecture.png",
    "IAssureClaimArchitecture": "./components/Sidebar/Icons/IAssureClaimarchitecture.png",
    "AIForceArchitecture": "./components/Sidebar/Icons/AIForcearchitecture.png",
    "CitizenAdvisorArchitecture": "https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisor/technical-architecture/CitizenAdvisorArchitecture.png"
  },
  "solutionsBenefits": {
    "citizenBenefits": "https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisor/benefits/CitizenAdvisorBenefits.png",
    "smartRecruitBenefits": "https://aiml-convai.s3.amazonaws.com/portal-slides/smart-recruit/benefits/SmartRecruitBenefits.png"
  },
  "adoption": {
    "citizenAdoption": "https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisor/industry-adoption/CitizenAdvisorAdoption.png",
    "smartRecruitAdoption": "https://aiml-convai.s3.amazonaws.com/portal-slides/smart-recruit/industry-adoption/SmartRecruitAdoption.png"
  }
}






import URLJson from './URLJson.json';

function getSolutionFlowURLs(type, count) {
  const { baseURL, paths } = URLJson.solutionFlows;
  const path = paths[type];
  if (!path) return [];

  return Array.from({ length: count }, (_, i) => `${baseURL}/${path}${i + 1}.png`);
}

const images = {
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
  PredictiveAsset: require('./components/Cards/CardsImages/card11.jpg')
};

const videos = {
  EmailEARDemo: 'https://aiml-convai.s3.amazonaws.com/demovideos/Email-EAR_Demo_new.mp4',
  SignatureExtractionDemo: 'https://aiml-convai.s3.amazonaws.com/demovideos/Sign_Verification_New.mp4',
  IntelligentAssistDemo: 'https://aiml-convai.s3.amazonaws.com/demovideos/Intelligent_Assist-QnA_DemoVideo_new.mp4',
  CaseIntelligenceDemo: 'https://aiml-convai.s3.amazonaws.com/demovideos/Case-Intelligence_demo.mp4',
  CodeGReatDemo: 'https://aiml-convai.s3.amazonaws.com/demovideos/CodeGreat_Demo_new.mp4',
  SmartRecruitDemo: 'https://aiml-convai.s3.amazonaws.com/demovideos/SmartRecruit_IvAssist_Demo.mp4',
  CitizenAdvisorDemo: 'https://aiml-convai.s3.amazonaws.com/demovideos/Citizen_Advisor-Demo1.mp4'
};

const solutionFlows = {
  ...getSolutionFlowURLs('CitizenAdvisor', URLJson.solutionFlows.count),
  ...getSolutionFlowURLs('SmartRecruit', 2)
};

const architectures = {
  IntelligentAssistArchitecture: require('./components/Sidebar/Icons/IntelligentAssistarchitecture.png'),
  EmailEARArchitecture: require('./components/Sidebar/Icons/EmailEARarchitecture.png'),
  CaseIntelligenceArchitecture: require('./components/Sidebar/Icons/CaseIntelligencearchitecture.png'),
  SmartRecruitArchitecture: 'https://aiml-convai.s3.amazonaws.com/portal-slides/smart-recruit/technical-architecture/SmartRecruitArchitecture.png',
  AssistantEvArchitecture: require('./components/Sidebar/Icons/AssistantEvachitecture.png'),
  IAssureClaimArchitecture: require('./components/Sidebar/Icons/IAssureClaimarchitecture.png'),
  AIForceArchitecture: require('./components/Sidebar/Icons/AIForcearchitecture.png'),
  CitizenAdvisorArchitecture: 'https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisor/technical-architecture/CitizenAdvisorArchitecture.png'
};

const solutionsBenefits = {
  citizenBenefits: 'https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisor/benefits/CitizenAdvisorBenefits.png',
  smartRecruitBenefits: 'https://aiml-convai.s3.amazonaws.com/portal-slides/smart-recruit/benefits/SmartRecruitBenefits.png'
};

const adoption = {
  citizenAdoption: 'https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisor/industry-adoption/CitizenAdvisorAdoption.png',
  smartRecruitAdoption: 'https://aiml-convai.s3.amazonaws.com/portal-slides/smart-recruit/industry-adoption/SmartRecruitAdoption.png'
};

export { images, videos, solutionFlows, architectures, solutionsBenefits, adoption };











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

import { images, videos, solutionFlows, architectures, solutionsBenefits, adoption } from './AssetImports';

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
        ? card.content.description.map(desc => typeof desc === 'string' ? URLJson.descriptions[desc] : null)
        : (typeof card.content.description === 'string' ? [URLJson.descriptions[card.content.description]] : []),
      benefitsFlow: Array.isArray(card.content.benefits)
        ? card.content.benefits.map(benefit => typeof benefit === 'string' ? solutionsBenefits[benefit] : null)
        : (typeof card.content.benefits === 'string' ? [solutionsBenefits[card.content.benefits]] : []),
      adoptionFlow: Array.isArray(card.content.adoption)
        ? card.content.adoption.map(adopt => typeof adopt === 'string' ? adoption[adopt] : null)
        : (typeof card.content.adoption === 'string' ? [adoption[card.content.adoption]] : []),
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
