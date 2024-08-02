function mapAssets(card) {
  const getImage = (key) => key ? images[key.split('.').pop()] : null;
  const getVideo = (key) => key ? videos[key.split('.').pop()] : null;
  const getSolutionFlows = (keys) => Array.isArray(keys) ? keys.map(key => solutionFlows[key.split('.').pop()]) : [];
  const getArchitectures = (keys) => Array.isArray(keys) ? keys.map(key => architectures[key.split('.').pop()]) : [];
  const getDescriptions = (keys) => Array.isArray(keys) ? keys.map(key => descriptions[key.split('.').pop()]) : [];
  const getBenefits = (keys) => Array.isArray(keys) ? keys.map(key => solutionsBenefits[key.split('.').pop()]) : [];
  const getAdoption = (keys) => Array.isArray(keys) ? keys.map(key => adoption[key.split('.').pop()]) : [];

  return {
    ...card,
    imageUrl: getImage(card.imageUrl),
    content: {
      ...card.content,
      solutionFlow: getSolutionFlows(card.content.solutionFlow),
      demo: getVideo(card.content.demo),
      techArchitecture: getArchitectures(card.content.techArchitecture),
      descriptionFlow: getDescriptions(card.content.description),
      benefitsFlow: getBenefits(card.content.benefits),
      adoptionFlow: getAdoption(card.content.adoption),
    },
  };
}
