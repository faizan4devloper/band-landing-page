const S3_BASE_URL = 'https://aiml-convai.s3.amazonaws.com/portal-slides';

const generateS3Url = (category, subcategory, filename) => 
  `${S3_BASE_URL}/${category}/${subcategory}/${filename}`;

export const images = {
  IntelligentAss: require('./components/Cards/CardsImages/card3.jpg'),
  EmailEAR: require('./components/Cards/CardsImages/card1.jpg'),
  CaseIntelligence: require('./components/Cards/CardsImages/card4.jpg'),
  // ...
};

// Generate dynamic URLs for CitizenAdvisor category
export const dynamicAssets = {
  citizenDescription: generateS3Url('citizenadvisor', 'description', 'CitizenAdvisorDescription.png'),
  citizenBenefits: generateS3Url('citizenadvisor', 'benefits', 'CitizenAdvisorBenefits.png'),
  citizenAdoption: generateS3Url('citizenadvisor', 'industry-adoption', 'CitizenAdvisorAdoption.png'),
  CitizenAdvisorFlow1: generateS3Url('citizenadvisor', 'solutionFlows', 'CitizenAdvisorFlow1.png'),
  CitizenAdvisorFlow2: generateS3Url('citizenadvisor', 'solutionFlows', 'CitizenAdvisorFlow2.png'),
  CitizenAdvisorFlow3: generateS3Url('citizenadvisor', 'solutionFlows', 'CitizenAdvisorFlow3.png'),
  CitizenAdvisorFlow4: generateS3Url('citizenadvisor', 'solutionFlows', 'CitizenAdvisorFlow4.png'),
  CitizenAdvisorFlow5: generateS3Url('citizenadvisor', 'solutionFlows', 'CitizenAdvisorFlow5.png'),
  CitizenAdvisorArchitecture: generateS3Url('citizenadvisor', 'technical-architecture', 'CitizenAdvisorArchitecture.png'),
};

// Videos
export const videos = {
  EmailEARDemo: 'https://aiml-convai.s3.amazonaws.com/demovideos/Email-EAR_Demo_new.mp4',
  // ...
};

// Similarly, generate dynamic URLs for other categories if needed
export const smartRecruitAssets = {
  smartRecruitDescription1: generateS3Url('smart-recruit', 'description', 'SmartRecruitDescription1.png'),
  // ...
};

// Include other categories as needed




import { dynamicAssets } from './AssetImports';

function mapAssets(card) {
  const getDynamicAsset = (key) => dynamicAssets[key] || null;

  return {
    ...card,
    imageUrl: getDynamicAsset(card.imageUrl),
    content: {
      ...card.content,
      solutionFlow: card.content.solutionFlow.map(getDynamicAsset),
      demo: [getDynamicAsset(card.content.demo)],
      techArchitecture: card.content.techArchitecture.map(getDynamicAsset),
      descriptionFlow: card.content.description.map(getDynamicAsset),
      benefitsFlow: card.content.benefits.map(getDynamicAsset),
      adoptionFlow: card.content.adoption.map(getDynamicAsset),
    },
  };
}




{
  "imageUrl": "CitizenAdvisor",
  "title": "Citizen Advisor",
  "description": "An experience transformation from disconnected silos information to an intuitive, personalized revelations",
  "industry": "GOVT",
  "businessFunction": "Customer Experience",
  "content": {
    "description": ["citizenDescription"],
    "solutionFlow": [
      "CitizenAdvisorFlow1", 
      "CitizenAdvisorFlow2", 
      "CitizenAdvisorFlow3", 
      "CitizenAdvisorFlow4", 
      "CitizenAdvisorFlow5"
    ],
    "demo": "CitizenAdvisorDemo",
    "techArchitecture": ["CitizenAdvisorArchitecture"],
    "benefits": ["citizenBenefits"],
    "adoption": ["citizenAdoption"]
  }
}
