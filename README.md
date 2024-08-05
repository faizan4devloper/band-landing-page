// AssetImports.js
import axios from 'axios';
import IntelligentAssist from './components/Cards/CardsImages/card3.jpg';
import EmailEAR from './components/Cards/CardsImages/card1.jpg';
import CaseIntelligence from './components/Cards/CardsImages/card4.jpg';
// ... other imports

export const images = {
  IntelligentAssist,
  EmailEAR,
  CaseIntelligence,
  // ... other images
};

export const videos = {
  EmailEARDemo: 'https://aiml-convai.s3.amazonaws.com/demovideos/Email-EAR_Demo_new.mp4',
  SignatureExtractionDemo: 'https://aiml-convai.s3.amazonaws.com/demovideos/Sign_Verification_New.mp4',
  // ... other videos
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
import { images, videos, fetchAssets } from './AssetImports';

async function mapAssets(card) {
  const assets = await fetchAssets();
  const assetKey = card.title.replace(/\s+/g, '');

  const data = assets[`${assetKey}description`] || {};

  return {
    ...card,
    imageUrl: images[card.imageUrl] || 'defaultImagePath.jpg', // Fallback to a default image
    content: {
      ...card.content,
      solutionFlow: data.solutionFlow || null,
      demo: videos[card.content.demo] || null,
      techArchitecture: data.techArchitecture || null,
      descriptionFlow: data.description || null,
      benefitsFlow: data.benefits || null,
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
    // ... other cards
  ]);

  return cardsData;
}



{
  "imageUrl": "CitizenAdvisor", 
  "title": "Citizen Advisor",
  "description": "An experience transformation from disconnected silos of information to intuitive, personalized revelations.",
  "industry": "GOVT",
  "businessFunction": "Customer Experience",
  "content": {
    "description": "CitizenAdvisordescription",
    "solutionFlow": "CitizenAdvisorsolutionFlow",
    "demo": "CitizenAdvisorDemo",
    "techArchitecture": "CitizenAdvisortechArchitecture",
    "benefits": "CitizenAdvisorbenefits",
    "adoption": "CitizenAdvisoradoption"
  }
}
