Uncaught TypeError: Cannot read properties of undefined (reading 'split')
    at mapAssets (data.js:32:1)
    at ./src/data.js (data.js:37:1)
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
      solutionFlow: card.content.solutionFlow ? solutionFlows[card.content.solutionFlow.split('.').pop()] : null,
      demo: card.content.demo ? videos[card.content.demo.split('.').pop()] : null,
      techArchitecture: card.content.techArchitecture ? architectures[card.content.techArchitecture.split('.').pop()] : null,
      descriptionImage: card.content.descriptionImage ? images[card.content.descriptionImage.split('.').pop()] : null,
    },
  };
}
