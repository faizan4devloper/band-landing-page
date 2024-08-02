function mapAssets(card) {
  return {
    ...card,
    imageUrl: card.imageUrl ? images[card.imageUrl.split('.').pop()] : null,
    content: {
      ...card.content,
      solutionFlow: Array.isArray(card.content.solutionFlow)
        ? card.content.solutionFlow.map(flow => typeof flow === 'string' ? solutionFlows[flow.split('.').pop()] : null)
        : [], // Default to an empty array if not an array
      demo: card.content.demo ? videos[card.content.demo.split('.').pop()] : null,
      techArchitecture: Array.isArray(card.content.techArchitecture)
        ? card.content.techArchitecture.map(arch => typeof arch === 'string' ? architectures[arch.split('.').pop()] : null)
        : [],
      descriptionFlow: Array.isArray(card.content.description)
        ? card.content.description.map(desc => typeof desc === 'string' ? descriptions[desc.split('.').pop()] : null)
        : [],
      benefitsFlow: Array.isArray(card.content.benefits)
        ? card.content.benefits.map(benefit => typeof benefit === 'string' ? solutionsBenefits[benefit.split('.').pop()] : null)
        : [],
      adoptionFlow: Array.isArray(card.content.adoption)
        ? card.content.adoption.map(adopt => typeof adopt === 'string' ? adoption[adopt.split('.').pop()] : null)
        : [],
    },
  };
}
