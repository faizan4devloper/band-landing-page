// data.js
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

import { images, videos, solutionFlows, architectures, descriptions, solutionsBenefits, adoption } from './AssetImport';

function mapAssets(card) {
  return {
    ...card,
    imageUrl: images[card.imageUrl] || null,
    content: {
      ...card.content,
      solutionFlow: Array.isArray(card.content.solutionFlow)
        ? card.content.solutionFlow.map(flow => solutionFlows[flow] || null)
        : (typeof card.content.solutionFlow === 'string' ? [solutionFlows[card.content.solutionFlow] || null] : []),
      demo: videos[card.content.demo] || null,
      techArchitecture: Array.isArray(card.content.techArchitecture)
        ? card.content.techArchitecture.map(arch => architectures[arch] || null)
        : (typeof card.content.techArchitecture === 'string' ? [architectures[card.content.techArchitecture] || null] : []),
      descriptionFlow: Array.isArray(card.content.description)
        ? card.content.description.map(desc => descriptions[desc] || null)
        : (typeof card.content.description === 'string' ? [descriptions[card.content.description] || null] : []),
      benefitsFlow: Array.isArray(card.content.benefits)
        ? card.content.benefits.map(benefit => solutionsBenefits[benefit] || null)
        : (typeof card.content.benefits === 'string' ? [solutionsBenefits[card.content.benefits] || null] : []),
      adoptionFlow: Array.isArray(card.content.adoption)
        ? card.content.adoption.map(adopt => adoption[adopt] || null)
        : (typeof card.content.adoption === 'string' ? [adoption[card.content.adoption] || null] : []),
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
