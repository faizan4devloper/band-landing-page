// AssetImports.js
import urldata from 'https://aiml-convai.s3.amazonaws.com/portal-slides/URLJson.json'; // Adjust the path as necessary

// Function to get URLs from urldata.json
export function getAssetUrl(type, key) {
  const asset = urldata[type]?.find(item => item.key === key);
  return asset ? asset.url : null;
}

// Example usage
export const images = {
  CitizenAdvisor: getAssetUrl('images', 'CitizenAdvisor'),
  // Add other assets similarly...
};

export const videos = {
  CitizenAdvisorDemo: getAssetUrl('videos', 'CitizenAdvisorDemo'),
  // Add other assets similarly...
};

// Similarly update solutionFlows, architectures, descriptions, solutionsBenefits, adoption









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


import { getAssetUrl } from './AssetImports';

function mapAssets(card) {
  return {
    ...card,
    imageUrl: card.imageUrl ? getAssetUrl('images', card.imageUrl) : null,
    content: {
      ...card.content,
      solutionFlow: Array.isArray(card.content.solutionFlow)
        ? card.content.solutionFlow.map(flow => getAssetUrl('solutionFlows', flow))
        : [],
      demo: card.content.demo ? getAssetUrl('videos', card.content.demo) : null,
      techArchitecture: Array.isArray(card.content.techArchitecture)
        ? card.content.techArchitecture.map(arch => getAssetUrl('architectures', arch))
        : [],
      descriptionFlow: Array.isArray(card.content.description)
        ? card.content.description.map(desc => getAssetUrl('descriptions', desc))
        : [],
      benefitsFlow: Array.isArray(card.content.benefits)
        ? card.content.benefits.map(benefit => getAssetUrl('solutionsBenefits', benefit))
        : [],
      adoptionFlow: Array.isArray(card.content.adoption)
        ? card.content.adoption.map(adopt => getAssetUrl('adoption', adopt))
        : [],
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




ERROR
Failed to fetch dynamically imported module: https://aiml-convai.s3.amazonaws.com/portal-slides/URLJson.json
TypeError: Failed to fetch dynamically imported module: https://aiml-convai.s3.amazonaws.com/portal-slides/URLJson.json
