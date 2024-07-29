// Your file with the mapAssets function

const { images, videos, solutionFlows, architectures, benefitsImages } = require('./AssetImports');

function mapAssets(card) {
  return {
    ...card,
    imageUrl: images[card.imageUrl?.split('.').pop()],
    content: {
      ...card.content,
      solutionFlow: solutionFlows[card.content.solutionFlow?.split('.').pop()],
      demo: videos[card.content.demo?.split('.').pop()],
      techArchitecture: architectures[card.content.techArchitecture?.split('.').pop()],
      benefitsImg: benefitsImages[card.content.benefitsImg?.split('.').pop()],
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
