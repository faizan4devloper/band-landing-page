const { images, videos, solutionFlows, architectures, descriptions } = require('./AssetImports');

function mapAssets(card) {
  return {
    ...card,
    imageUrl: images[card.imageUrl.split('.').pop()],
    content: {
      ...card.content,
      solutionFlow: solutionFlows[card.content.solutionFlow.split('.').pop()],
      demo: videos[card.content.demo.split('.').pop()],
      techArchitecture: architectures[card.content.techArchitecture.split('.').pop()],
      descriptionFlow: descriptions[card.content.CitizenAdvisorDesc.split('.').pop()],
    },
  };
}
