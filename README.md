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

const fetchAssets = async () => {
  try {
    const response = await axios.get('https://aiml-convai.s3.amazonaws.com/portal-slides/URLJson.json');
    return response.data;
  } catch (error) {
    console.error('Error fetching assets:', error);
    return {};
  }
};

const mapAssets = async (card) => {
  const assetData = await fetchAssets();
  const assetKey = card.title.replace(/\s+/g, '');
  const data = assetData[assetKey] || {};
  console.log('https://aiml-convai.s3.amazonaws.com/portal-slides/URLJson.json')

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
};

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

// CitizenAdvisor.json
{
  "imageUrl": "CitizenAdvisor",
  "title": "Citizen Advisor",
  "description": "An experience transformation from disconnected silos information to an intuitive, personalized revelations",
  "industry": "GOVT",
  "businessFunction": "Customer Experience",
  "content": {
    "description": ["citizenDescription"],
    "solutionFlow": ["citizenAdvisorSolution" ],
    "demo": "CitizenAdvisorDemo",
    "techArchitecture": ["CitizenAdvisorArchitecture"],
    "benefits": ["citizenBenefits"],
    "adoption": ["citizenAdoption"]
  }
}



// URLJson.json from s3 bucket
{
    "CitizenAdvisor": {
      "benefits": ["https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisorv1/benefits/Slide8.png"],
      "techArchitecture": ["https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisorv1/techArchitecture/Slide7.png"],
      "solutionFlow": [
        "https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisorv1/solutionFlow/Slide6.png",
        "https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisorv1/solutionFlow/Slide3.png",
        "https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisorv1/solutionFlow/Slide5.png",
        "https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisorv1/solutionFlow/Slide2.png",
        "https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisorv1/solutionFlow/Slide4.png"
      ],
      "description": ["https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisorv1/description/Slide1.png"],
      "adoption": ["https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisorv1/adoption/Slide9.png"]
    },
    "CaseIntelligence": {
      "description": ["https://aiml-convai.s3.amazonaws.com/portal-slides/caseintelligence/description/Slide1.png"],
      "techArchitecture": ["https://aiml-convai.s3.amazonaws.com/portal-slides/caseintelligence/techArchitecture/Slide3.png"],
      "adoption": ["https://aiml-convai.s3.amazonaws.com/portal-slides/caseintelligence/adoption/Slide5.png"],
      "solutionFlow": ["https://aiml-convai.s3.amazonaws.com/portal-slides/caseintelligence/solutionFlow/Slide2.png"],
      "benefits": ["https://aiml-convai.s3.amazonaws.com/portal-slides/caseintelligence/benefits/Slide4.png"]
    },
    "IntelligentAssist": {
      "description": ["https://aiml-convai.s3.amazonaws.com/portal-slides/intelligentassist/description/Slide1.png"],
      "techArchitecture": ["https://aiml-convai.s3.amazonaws.com/portal-slides/intelligentassist/techArchitecture/Slide3.png"],
      "adoption": ["https://aiml-convai.s3.amazonaws.com/portal-slides/intelligentassist/adoption/Slide5.png"],
      "solutionFlow": ["https://aiml-convai.s3.amazonaws.com/portal-slides/intelligentassist/solutionFlow/Slide2.png"],
      "benefits": ["https://aiml-convai.s3.amazonaws.com/portal-slides/intelligentassist/benefits/Slide4.png"]
    },
    "SmartRecruit": {
      "adoption": ["https://aiml-convai.s3.amazonaws.com/portal-slides/smartrecruit/adoption/Slide7.png"],
      "benefits": ["https://aiml-convai.s3.amazonaws.com/portal-slides/smartrecruit/benefits/Slide6.png"],
      "description": [
        "https://aiml-convai.s3.amazonaws.com/portal-slides/smartrecruit/description/Slide1.png",
        "https://aiml-convai.s3.amazonaws.com/portal-slides/smartrecruit/description/Slide2.png"
      ],
      "solutionFlow": [
        "https://aiml-convai.s3.amazonaws.com/portal-slides/smartrecruit/solutionFlow/Slide3.png",
        "https://aiml-convai.s3.amazonaws.com/portal-slides/smartrecruit/solutionFlow/Slide4.png"
      ],
      "techArchitecture": ["https://aiml-convai.s3.amazonaws.com/portal-slides/smartrecruit/techArchitecture/Slide5.png"]
    },
    "EmailEAR": {
      "description": ["https://aiml-convai.s3.amazonaws.com/portal-slides/emailear/description/Slide1.png"],
      "techArchitecture": ["https://aiml-convai.s3.amazonaws.com/portal-slides/emailear/techArchitecture/Slide3.png"],
      "adoption": ["https://aiml-convai.s3.amazonaws.com/portal-slides/emailear/adoption/Slide5.png"],
      "solutionFlow": ["https://aiml-convai.s3.amazonaws.com/portal-slides/emailear/solutionFlow/Slide2.png"],
      "benefits": ["https://aiml-convai.s3.amazonaws.com/portal-slides/emailear/benefits/Slide4.png"]
    }
  }
