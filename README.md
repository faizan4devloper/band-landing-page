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
// Import other card JSON files similarly

const { images, videos, solutionFlows, architectures, descriptions } = require('./AssetImports');

function mapAssets(card) {
  return {
    ...card,
    imageUrl: card.imageUrl ? images[card.imageUrl.split('.').pop()] : null,
    content: {
      ...card.content,
      solutionFlow: card.content.solutionFlow ? solutionFlows[card.content.solutionFlow.split('.').pop()] : null,
      demo: card.content.demo ? videos[card.content.demo.split('.').pop()] : null,
      techArchitecture: card.content.techArchitecture ? architectures[card.content.techArchitecture.split('.').pop()] : null,
      descriptionFlow: card.content.CitizenAdvisorDesc ? descriptions[card.content.CitizenAdvisorDesc.split('.').pop()] : null,
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
  // Map other card JSON files similarly
];
