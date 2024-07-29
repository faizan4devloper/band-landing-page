// AssetsImports.js

// Existing imports...
export const images = {
  // existing image imports
};

export const videos = {
  // existing video imports
};

export const solutionFlows = {
  // existing solution flow imports
};

export const architectures = {
  // existing architecture imports
};

// Benefits Images
export const benefitsImages = {
  IntelligentAssistBenefits: require('./components/Sidebar/Icons/IntelligentAssistBenefits.png'),
  EmailEARBenefits: require('./components/Sidebar/Icons/EmailEARBenefits.png'),
  CaseIntelligenceBenefits: require('./components/Sidebar/Icons/CaseIntelligenceBenefits.png'),
  SmartRecruitBenefits: require('./components/Sidebar/Icons/SmartRecruitBenefits.png'),
  IAssureClaimBenefits: require('./components/Sidebar/Icons/IAssureClaimBenefits.png'),
  AssistantEVBenefits: require('./components/Sidebar/Icons/AssistantEVBenefits.png'),
  // add other benefits images similarly
};


{
  "title": "Intelligent Assist",
  "description": "Description for Intelligent Assist",
  "imageUrl": "IntelligentAss",
  "content": {
    "solutionFlow": "IntelligentAssistFlow",
    "demo": "demoVideo3",
    "techArchitecture": "IntelligentAssistArchitecture",
    "benefitsImg": "IntelligentAssistBenefits"
  }
}




// Your file with the mapAssets function

const { images, videos, solutionFlows, architectures, benefitsImages } = require('./AssetImports');

function mapAssets(card) {
  return {
    ...card,
    imageUrl: images[card.imageUrl.split('.').pop()],
    content: {
      ...card.content,
      solutionFlow: solutionFlows[card.content.solutionFlow.split('.').pop()],
      demo: videos[card.content.demo.split('.').pop()],
      techArchitecture: architectures[card.content.techArchitecture.split('.').pop()],
      benefitsImg: benefitsImages[card.content.benefitsImg.split('.').pop()],
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
