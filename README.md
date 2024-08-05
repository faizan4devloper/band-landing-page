
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

import { images, videos, fetchAssets } from './AssetImports';

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
  
  console.log(data)

  return {
    ...card,
    imageUrl: images[card.imageUrl] || 'defaultImagePath.jpg', // Use a default image if the specified one is missing
    content: {
      ...card.content,
      solutionFlow: data.solutionFlow,
      demo: videos[card.content.demo] || null,
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
  ]);

  return cardsData;
}





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

import { images, videos, fetchAssets } from './AssetImports';

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
  
  console.log(data)

  return {
    ...card,
    imageUrl: images[card.imageUrl] || 'defaultImagePath.jpg', // Use a default image if the specified one is missing
    content: {
      ...card.content,
      solutionFlow: data.solutionFlow,
      demo: videos[card.content.demo] || null,
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
  ]);

  return cardsData;
}



{
  "imageUrl": "CitizenAdvisor", 
  "title": "Citizen Advisor",
  "description": "An experience transformation from disconnected silos information to an intuitive, personalized revelations",
  "industry": "GOVT",
  "businessFunction": "Customer Experience",
  "content": {
    "description": ["CitizenAdvisordescription"], 
    "demo": "CitizenAdvisorDemo",  
    "techArchitecture": ["CitizenAdvisortechArchitecture"], 
    "benefits": ["benefits"],  
    "adoption": ["adoption"]  
  }
}
