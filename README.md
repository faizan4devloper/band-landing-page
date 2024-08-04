import axios from 'axios';
import { images, videos } from './AssetImports';

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

async function fetchAssets() {
  try {
    const response = await axios.get('https://aiml-convai.s3.amazonaws.com/portal-slides/URLJson.json');
    return response.data;
  } catch (error) {
    console.error('Error fetching assets:', error);
    return {}; // Return an empty object or handle the error as needed
  }
}

async function mapAssets(card) {
  const assetData = await fetchAssets();
  const assetKey = card.title.replace(/\s+/g, '');
  const data = assetData[assetKey] || {};

  return {
    ...card,
    imageUrl: card.imageUrl ? images[card.imageUrl] : null,
    content: {
      ...card.content,
      solutionFlow: data.solutionFlow || [],
      demo: card.content.demo ? videos[card.content.demo] : null,
      techArchitecture: data.techArchitecture || [],
      descriptionFlow: data.description || [],
      benefitsFlow: data.benefits || [],
      adoption: data.adoption || []
    }
  };
}

export async function getCardsData() {
  const cardData = [
    IntelligentAssist,
    EmailEAR,
    CaseIntelligence,
    SmartRecruit,
    IAssureClaim,
    AssistantEV,
    AutoWiseCompanion,
    CitizenAdvisor,
    FinCompetitor,
    SignatureExtraction,
    AiForce,
    ApiCase,
    AmsSupport,
    CodeGreat,
    AaigApi,
    ResponsibleGen,
    GraphData,
    PredictiveAsset
  ];

  const mappedCards = await Promise.all(cardData.map(mapAssets));
  return mappedCards;
}
