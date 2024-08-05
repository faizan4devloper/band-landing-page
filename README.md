function mapAssets(card) {
  return {
    ...card,
    imageUrl: card.imageUrl ? images[card.imageUrl.split('.').pop()] : null,
    content: {
      ...card.content,
      solutionFlow: Array.isArray(card.content.solutionFlow)
        ? card.content.solutionFlow.map(flow => typeof flow === 'string' ? solutionFlows[flow.split('.').pop()] : null)
        : [],
      demo: typeof card.content.demo === 'string'
        ? videos[card.content.demo.split('.').pop()]
        : null, // Ensure demo is a string before splitting
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
