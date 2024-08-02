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

import { images, videos, solutionFlows, architectures, descriptions, solutionsBenefits, adoption } from './AssetImports';

// Fetch urldata.json dynamically
async function fetchUrldata() {
  try {
    const response = await fetch('https://aiml-convai.s3.amazonaws.com/portal-slides/urldata.json'); // Adjust the path to your urldata.json
    if (!response.ok) {
      throw new Error(`HTTP error! Status: ${response.status}`);
    }
    return response.json();
  } catch (error) {
    console.error('Error fetching urldata.json:', error);
    throw error; // Re-throw to be handled where fetchUrldata is called
  }
}

export async function getCardsData() {
  try {
    const urldata = await fetchUrldata();

    function mapAssets(card) {
      return {
        ...card,
        imageUrl: card.imageUrl ? images[card.imageUrl] : null,
        content: {
          ...card.content,
          solutionFlow: Array.isArray(card.content.solutionFlow)
            ? card.content.solutionFlow.map(flow => urldata[flow] || solutionFlows[flow])
            : [],
          demo: card.content.demo ? (urldata[card.content.demo] || videos[card.content.demo]) : null,
          techArchitecture: Array.isArray(card.content.techArchitecture)
            ? card.content.techArchitecture.map(arch => urldata[arch] || architectures[arch])
            : [],
          descriptionFlow: Array.isArray(card.content.description)
            ? card.content.description.map(desc => urldata[desc] || descriptions[desc])
            : [],
          benefitsFlow: Array.isArray(card.content.benefits)
            ? card.content.benefits.map(benefit => urldata[benefit] || solutionsBenefits[benefit])
            : [],
          adoptionFlow: Array.isArray(card.content.adoption)
            ? card.content.adoption.map(adopt => urldata[adopt] || adoption[adopt])
            : [],
        },
      };
    }

    return [
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
  } catch (error) {
    console.error('Error initializing cards data:', error);
    return []; // Return an empty array in case of error
  }
}
