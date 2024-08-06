// AssetImports.js
import axios from 'axios';

export const images = {
  IntelligentAssist: require('./components/Cards/CardsImages/card3.jpg'),
  EmailEAR: require('./components/Cards/CardsImages/card1.jpg'),
  CaseIntelligence: require('./components/Cards/CardsImages/card4.jpg'),
  SmartRecruit: require('./components/Cards/CardsImages/card8.jpg'),
  ClaimAssist: require('./components/Cards/CardsImages/card9.jpg'),
  AssistantEV: require('./components/Cards/CardsImages/card10.jpg'),
  CitizenAdvisor: require('./components/Cards/CardsImages/citizenAdvisor.jpg'),
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
  AutoWiseCampanian: require('./components/Cards/CardsImages/autowiseCompanian.jpg'),
};

// Videos
export const videos = {
  EmailEARDemo: 'https://aiml-convai.s3.amazonaws.com/demovideos/Email-EAR_Demo_new.mp4',
  SignatureExtractionDemo: 'https://aiml-convai.s3.amazonaws.com/demovideos/Sign_Verification_New.mp4',
  IntelligentAssistDemo: 'https://aiml-convai.s3.amazonaws.com/demovideos/Intelligent_Assist-QnA_DemoVideo_new.mp4',
  CaseIntelligenceDemo: 'https://aiml-convai.s3.amazonaws.com/demovideos/Case-Intelligence_demo.mp4',
  CodeGReatDemo: 'https://aiml-convai.s3.amazonaws.com/demovideos/CodeGreat_Demo_new.mp4',
  SmartRecruitDemo: 'https://aiml-convai.s3.amazonaws.com/demovideos/SmartRecruit_IvAssist_Demo.mp4',
  CitizenAdvisorDemo: 'https://aiml-convai.s3.amazonaws.com/portal-videos/CitizenAdvisorDemo.mp4',
};


// Fetch and cache JSON data using Axios
export async function fetchAssets() {
  try {
    const response = await axios.get('https://aiml-convai.s3.amazonaws.com/portal-slides/urldata.json');
    return response.data;
  } catch (error) {
    console.error('Failed to fetch assets:', error);
    return {}; // Fallback to an empty object in case of error
  }
}




// data.js
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

import { images, videos, fetchAssets } from './AssetImports';

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
  };
  
  console.log(`Data for ${card.title}:`, data);

  return {
    ...card,
    imageUrl: images[card.imageUrl] || 'defaultImagePath.jpg', // Use a default image if the specified one is missing
    content: {
      ...card.content,
      solutionFlow: data.solutionFlow || null,
      demo: videos[card.content.demo] || null,
      techArchitecture: data.techArchitecture || null,
      description: data.description || null,
      benefits: data.benefits || null,
      adoption: data.adoption || null,
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
  ]);

  return cardsData;
}




{
  "imageUrl": "EmailEAR",
  "title": "Email EAR",
  "description": "Transform customer support process by automating email analysis, and providing crafted thoughtful response to incoming emails",
  "industry": "All",
  "businessFunction": "Customer Support",
  "content": {
    "description": "EmailEardescription",
    "solutionFlow": "EmailEarsolutionFlow", 
    "demo": "EmailEARDemo",
    "techArchitecture": "EmailEartechArchitecture",
    "benefits": "EmailEarbenefits",
    "adoption": "EmailEaradoption"
  }
}


{"CitizenAdvisorbenefits": ["https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisorv1/benefits/Slide8.JPG"], "CitizenAdvisortechArchitecture": ["https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisorv1/techArchitecture/Slide7.JPG"], "CitizenAdvisorsolutionFlow": ["https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisorv1/solutionFlow/Slide6.JPG", "https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisorv1/solutionFlow/Slide3.JPG", "https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisorv1/solutionFlow/Slide5.JPG", "https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisorv1/solutionFlow/Slide2.JPG", "https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisorv1/solutionFlow/Slide4.JPG"], "CitizenAdvisordescription": ["https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisorv1/description/Slide1.JPG"], "CitizenAdvisoradoption": ["https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisorv1/adoption/Slide9.JPG"], "CaseIntelligencedescription": ["https://aiml-convai.s3.amazonaws.com/portal-slides/caseintelligence/description/Slide1.JPG"], "CaseIntelligencetechArchitecture": ["https://aiml-convai.s3.amazonaws.com/portal-slides/caseintelligence/techArchitecture/Slide3.JPG"], "CaseIntelligenceadoption": ["https://aiml-convai.s3.amazonaws.com/portal-slides/caseintelligence/adoption/Slide5.JPG"], "CaseIntelligencesolutionFlow": ["https://aiml-convai.s3.amazonaws.com/portal-slides/caseintelligence/solutionFlow/Slide2.JPG"], "CaseIntelligencebenefits": ["https://aiml-convai.s3.amazonaws.com/portal-slides/caseintelligence/benefits/Slide4.JPG"], "IntelligentAssistdescription": ["https://aiml-convai.s3.amazonaws.com/portal-slides/intelligentassist/description/Slide1.JPG"], "IntelligentAssisttechArchitecture": ["https://aiml-convai.s3.amazonaws.com/portal-slides/intelligentassist/techArchitecture/Slide3.JPG"], "IntelligentAssistadoption": ["https://aiml-convai.s3.amazonaws.com/portal-slides/intelligentassist/adoption/Slide5.JPG"], "IntelligentAssistsolutionFlow": ["https://aiml-convai.s3.amazonaws.com/portal-slides/intelligentassist/solutionFlow/Slide2.JPG"], "IntelligentAssistbenefits": ["https://aiml-convai.s3.amazonaws.com/portal-slides/intelligentassist/benefits/Slide4.JPG"], "SmartRecruitadoption": ["https://aiml-convai.s3.amazonaws.com/portal-slides/smartrecruit/adoption/Slide7.JPG"], "SmartRecruitbenefits": ["https://aiml-convai.s3.amazonaws.com/portal-slides/smartrecruit/benefits/Slide6.JPG"], "SmartRecruitdescription": ["https://aiml-convai.s3.amazonaws.com/portal-slides/smartrecruit/description/Slide1.JPG", "https://aiml-convai.s3.amazonaws.com/portal-slides/smartrecruit/description/Slide2.JPG"], "SmartRecruitsolutionFlow": ["https://aiml-convai.s3.amazonaws.com/portal-slides/smartrecruit/solutionFlow/Slide3.JPG", "https://aiml-convai.s3.amazonaws.com/portal-slides/smartrecruit/solutionFlow/Slide4.JPG"], "SmartRecruittechArchitecture": ["https://aiml-convai.s3.amazonaws.com/portal-slides/smartrecruit/techArchitecture/Slide5.JPG"], "EmailEardescription": ["https://aiml-convai.s3.amazonaws.com/portal-slides/emailear/description/Slide1.JPG"], "EmailEartechArchitecture": ["https://aiml-convai.s3.amazonaws.com/portal-slides/emailear/techArchitecture/Slide3.JPG"], "EmailEaradoption": ["https://aiml-convai.s3.amazonaws.com/portal-slides/emailear/adoption/Slide5.JPG"], "EmailEarsolutionFlow": ["https://aiml-convai.s3.amazonaws.com/portal-slides/emailear/solutionFlow/Slide2.JPG"], "EmailEarbenefits": ["https://aiml-convai.s3.amazonaws.com/portal-slides/emailear/benefits/Slide4.JPG"], "ClaimAssistadoption": ["https://aiml-convai.s3.amazonaws.com/portal-slides/claimassist/adoption/Slide7.JPG"], "ClaimAssistbenefits": ["https://aiml-convai.s3.amazonaws.com/portal-slides/claimassist/benefits/Slide6.JPG"], "ClaimAssistdescription": ["https://aiml-convai.s3.amazonaws.com/portal-slides/claimassist/description/Slide1.JPG"], "ClaimAssistsolutionFlow": ["https://aiml-convai.s3.amazonaws.com/portal-slides/claimassist/solutionFlow/Slide3.JPG", "https://aiml-convai.s3.amazonaws.com/portal-slides/claimassist/solutionFlow/Slide2.JPG", "https://aiml-convai.s3.amazonaws.com/portal-slides/claimassist/solutionFlow/Slide4.JPG"], "ClaimAssisttechArchitecture": ["https://aiml-convai.s3.amazonaws.com/portal-slides/claimassist/techArchitecture/Slide5.JPG"], "IFCdescription": ["https://aiml-convai.s3.amazonaws.com/portal-slides/emailear/ifc/description/Slide1.JPG"], "IFCtechArchitecture": ["https://aiml-convai.s3.amazonaws.com/portal-slides/emailear/ifc/techArchitecture/Slide3.JPG"], "IFCadoption": ["https://aiml-convai.s3.amazonaws.com/portal-slides/emailear/ifc/adoption/Slide5.JPG"], "IFCsolutionFlow": ["https://aiml-convai.s3.amazonaws.com/portal-slides/emailear/ifc/solutionFlow/Slide2.JPG"], "IFCbenefits": ["https://aiml-convai.s3.amazonaws.com/portal-slides/emailear/ifc/benefits/Slide4.JPG"], "CitizenAdvisordemo": ["https://aiml-convai.s3.amazonaws.com/portal-videos/CitizenAdvisorDemo.mp4"], "CaseIntelligencedemo": ["https://aiml-convai.s3.amazonaws.com/portal-videos/CaseIntelligenceDemo.mp4"], "IntelligentAssistdemo": ["https://aiml-convai.s3.amazonaws.com/portal-videos/IntelligentAssistDemo.mp4"], "SmartRecruitdemo": ["https://aiml-convai.s3.amazonaws.com/portal-videos/SmartRecruitDemo.mp4"], "EmaiEardemo": ["https://aiml-convai.s3.amazonaws.com/portal-videos/EmailEARDemo.mp4"], "claimassistdemo": ["https://aiml-convai.s3.amazonaws.com/portal-videos/claimassist.mp4"], "ifcdemo": ["https://aiml-convai.s3.amazonaws.com/portal-videos/ifc.mp4"]}
