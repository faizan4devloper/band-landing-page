function mapAssets(card) {
  const getDynamicAsset = (key) => dynamicAssets[key] || null;

  const getArray = (data) => Array.isArray(data) ? data : [data]; // Ensure data is an array

  return {
    ...card,
    imageUrl: getDynamicAsset(card.imageUrl),
    content: {
      ...card.content,
      solutionFlow: getArray(card.content.solutionFlow).map(getDynamicAsset),
      demo: [getDynamicAsset(card.content.demo)],
      techArchitecture: getArray(card.content.techArchitecture).map(getDynamicAsset),
      descriptionFlow: getArray(card.content.description).map(getDynamicAsset),
      benefitsFlow: getArray(card.content.benefits).map(getDynamicAsset),
      adoptionFlow: getArray(card.content.adoption).map(getDynamicAsset),
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
    "solutionFlow": ["CitizenAdvisorFlow1", "CitizenAdvisorFlow2"],
    "demo": "CitizenAdvisorDemo",
    "techArchitecture": ["CitizenAdvisorArchitecture"],
    "benefits": ["citizenBenefits"],
    "adoption": ["citizenAdoption"]
  }
}
