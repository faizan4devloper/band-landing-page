function mapAssets(card) {
  const getImage = (key) => (typeof key === 'string' ? images[key.split('.').pop()] : null);
  const getVideo = (key) => (typeof key === 'string' ? videos[key.split('.').pop()] : null);
  const getSolutionFlows = (keys) => Array.isArray(keys) ? keys.map(key => (typeof key === 'string' ? solutionFlows[key.split('.').pop()] : null)) : (typeof keys === 'string' ? [solutionFlows[keys.split('.').pop()]] : []);
  const getArchitectures = (keys) => Array.isArray(keys) ? keys.map(key => (typeof key === 'string' ? architectures[key.split('.').pop()] : null)) : (typeof keys === 'string' ? [architectures[keys.split('.').pop()]] : []);
  const getDescriptions = (keys) => Array.isArray(keys) ? keys.map(key => (typeof key === 'string' ? descriptions[key.split('.').pop()] : null)) : (typeof keys === 'string' ? [descriptions[keys.split('.').pop()]] : []);
  const getBenefits = (keys) => Array.isArray(keys) ? keys.map(key => (typeof key === 'string' ? solutionsBenefits[key.split('.').pop()] : null)) : (typeof keys === 'string' ? [solutionsBenefits[keys.split('.').pop()]] : []);
  const getAdoption = (keys) => Array.isArray(keys) ? keys.map(key => (typeof key === 'string' ? adoption[key.split('.').pop()] : null)) : (typeof keys === 'string' ? [adoption[keys.split('.').pop()]] : []);

  return {
    ...card,
    imageUrl: getImage(card.imageUrl),
    content: {
      ...card.content,
      solutionFlow: getSolutionFlows(card.content.solutionFlow),
      demo: [getVideo(card.content.demo)], // Ensure demo is always an array
      techArchitecture: getArchitectures(card.content.techArchitecture),
      descriptionFlow: getDescriptions(card.content.description),
      benefitsFlow: getBenefits(card.content.benefits),
      adoptionFlow: getAdoption(card.content.adoption),
    },
  };
}
