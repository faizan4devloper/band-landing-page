// data.js
import { images, videos } from './AssetImports';

async function fetchAssets() {
  const response = await fetch('https://aiml-convai.s3.amazonaws.com/portal-slides/URLJson.json');
  return await response.json();
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
