Uncaught TypeError: card.content.adoption.split is not a function
    at mapAssets (data.js:37:1)
    at ./src/data.js (data.js:43:1)
    at options.factory (react refresh:6:1)
    at __webpack_require__ (bootstrap:22:1)
    at fn (hot module replacement:61:1)
    at ./src/components/Sidebar/SideBarPage.js (SideBar.js:51:1)
    at options.factory (react refresh:6:1)
    at __webpack_require__ (bootstrap:22:1)
    at fn (hot module replacement:61:1)
    at ./src/App.js (SolutionFlow.svg:31:1)





function mapAssets(card) {
  return {
    ...card,
    imageUrl: card.imageUrl ? images[card.imageUrl.split('.').pop()] : null,
    content: {
      ...card.content,
      solutionFlow: Array.isArray(card.content.solutionFlow) 
        ? card.content.solutionFlow.map(flow => solutionFlows[flow.split('.').pop()]) 
        : [],
      demo: card.content.demo ? videos[card.content.demo.split('.').pop()] : null,
      techArchitecture: card.content.techArchitecture ? architectures[card.content.techArchitecture.split('.').pop()] : null,
      descriptionFlow: card.content.description ? descriptions[card.content.description.split('.').pop()] : null,
      benefitsFlow: card.content.benefits ? solutionsBenefits[card.content.benefits.split('.').pop()] : null,
      adoptionFlow: typeof card.content.adoption === 'string' ? adoption[card.content.adoption.split('.').pop()] : null,
    },
  };
}



{
  "imageUrl": "images.CitizenAdvisor",
  "title": "Citizen Advisor",
  "description": "An experience transformation from disconnected silos information to an intuitive, personalized revelations",
  "industry": "GOVT",
  "businessFunction": "Customer Experience",
  "content": {
    "description": "descriptions.DescriptionDemo",
    "solutionFlow": ["solutionFlows.CitizenAdvisorFlow1", "solutionFlows.CitizenAdvisorFlow2", "solutionFlows.CitizenAdvisorFlow3", "solutionFlows.CitizenAdvisorFlow4"],
    "demo": "videos.demoVideo5",
    "techArchitecture": "architectures.CitizenAdvisorArchitecture",
    "benefits": "solutionsBenefits.citizenBenefits",
    "adoption": "adoption.citizenAdoption"
  }
}



// AssetImports.js
export const adoption = {
  citizenAdoption: '/path/to/citizenAdoption.png',
  // Other adoption paths
};
