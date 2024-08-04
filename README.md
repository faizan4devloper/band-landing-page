import axios from 'axios';
import { images, videos } from './AssetImports';

// Import JSON data for cards
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

// Function to fetch asset data from S3
const fetchAssets = async () => {
  try {
    const response = await axios.get('https://aiml-convai.s3.amazonaws.com/portal-slides/URLJson.json');
    return response.data;
  } catch (error) {
    console.error('Error fetching assets:', error);
    return {};
  }
};

// Function to map assets to card data
const mapAssets = async (card) => {
  try {
    const assetData = await fetchAssets();
    const assetKey = card.title.replace(/\s+/g, ''); // Clean the card title to match the asset key
    const data = assetData[assetKey] || {}; // Retrieve asset data using the cleaned key

    console.log('Mapping assets for:', card.title);
    console.log('Fetched asset data:', data);

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
  } catch (error) {
    console.error('Error mapping assets for card:', card.title, error);
    return card; // Return the original card data in case of an error
  }
};

// Function to get all card data
export const getCardsData = async () => {
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
};





{
  "imageUrl": "CitizenAdvisor",
  "title": "Citizen Advisor",
  "description": "An experience transformation from disconnected silos information to an intuitive, personalized revelations",
  "industry": "GOVT",
  "businessFunction": "Customer Experience",
  "content": {
    "description": ["citizenDescription"],
    "solutionFlow": ["citizenAdvisorSolution"],
    "demo": "CitizenAdvisorDemo",
    "techArchitecture": ["CitizenAdvisorArchitecture"],
    "benefits": ["citizenBenefits"],
    "adoption": ["citizenAdoption"]
  }
}
