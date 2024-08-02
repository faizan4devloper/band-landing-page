// AssetImport.js

// Dummy placeholders for images and other assets
export const images = {};
export const videos = {};
export const solutionFlows = {};
export const architectures = {};
export const descriptions = {};
export const solutionsBenefits = {};
export const adoption = {};



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

const { images, videos, solutionFlows, architectures, descriptions, solutionsBenefits, adoption } = require('./AssetImports');

const ASSET_URL = 'https://your-bucket-name.s3.amazonaws.com/urldata.json';

const fetchData = async (url) => {
  const response = await fetch(url);
  if (!response.ok) {
    throw new Error('Failed to fetch data');
  }
  return response.json();
};

const mapAssets = (card, data) => {
  const mapAssetUrls = (keys, data) => {
    return keys.map(key => (data[key] ? data[key][0] : null));
  };

  return {
    ...card,
    imageUrl: card.imageUrl ? mapAssetUrls([card.imageUrl], data)[0] : null,
    content: {
      ...card.content,
      solutionFlow: Array.isArray(card.content.solutionFlow)
        ? mapAssetUrls(card.content.solutionFlow, data)
        : [],
      demo: card.content.demo ? mapAssetUrls([card.content.demo], data)[0] : null,
      techArchitecture: Array.isArray(card.content.techArchitecture)
        ? mapAssetUrls(card.content.techArchitecture, data)
        : [],
      description: Array.isArray(card.content.description)
        ? mapAssetUrls(card.content.description, data)
        : [],
      benefits: Array.isArray(card.content.benefits)
        ? mapAssetUrls(card.content.benefits, data)
        : [],
      adoption: Array.isArray(card.content.adoption)
        ? mapAssetUrls(card.content.adoption, data)
        : [],
    },
  };
};

export const loadCardsData = async () => {
  const data = await fetchData(ASSET_URL);

  return [
    mapAssets(IntelligentAssist, data),
    mapAssets(EmailEAR, data),
    mapAssets(CaseIntelligence, data),
    mapAssets(SmartRecruit, data),
    mapAssets(IAssureClaim, data),
    mapAssets(AssistantEV, data),
    mapAssets(AutoWiseCompanion, data),
    mapAssets(CitizenAdvisor, data),
    mapAssets(FinCompetitor, data),
    mapAssets(SignatureExtraction, data),
    mapAssets(AiForce, data),
    mapAssets(ApiCase, data),
    mapAssets(AmsSupport, data),
    mapAssets(CodeGreat, data),
    mapAssets(AaigApi, data),
    mapAssets(ResponsibleGen, data),
    mapAssets(GraphData, data),
    mapAssets(PredictiveAsset, data),
  ];
};
