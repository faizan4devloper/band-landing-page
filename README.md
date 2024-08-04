// AssetImports.js
export async function fetchUrldata() {
  try {
    const response = await fetch('https://aiml-convai.s3.amazonaws.com/portal-slides/URLJson.json');
    if (!response.ok) {
      throw new Error('Network response was not ok');
    }
    return await response.json();
  } catch (error) {
    console.error('Failed to fetch urldata.json:', error);
    return null;
  }
}

export function getAssetUrl(type, key, urldata) {
  const asset = urldata[type]?.find(item => item.key === key);
  return asset ? asset.url : null;
}

// Example usage
export async function fetchAssets() {
  const urldata = await fetchUrldata();
  if (!urldata) return null;

  return {
    images: {
      CitizenAdvisor: getAssetUrl('images', 'CitizenAdvisor', urldata),
      // Add other assets similarly...
    },
    videos: {
      CitizenAdvisorDemo: getAssetUrl('videos', 'CitizenAdvisorDemo', urldata),
      // Add other assets similarly...
    },
    // Add other categories similarly...
  };
}



// data.js

import IntelligentAssist from './CardsData/IntelligentAssist.json';
// Import other card data similarly...

import { fetchAssets } from './AssetImports';

export const cardsData = (async () => {
  const assets = await fetchAssets();
  if (!assets) return [];

  function mapAssets(card) {
    return {
      ...card,
      imageUrl: card.imageUrl ? assets.images[card.imageUrl] : null,
      content: {
        ...card.content,
        solutionFlow: Array.isArray(card.content.solutionFlow)
          ? card.content.solutionFlow.map(flow => assets.solutionFlows[flow])
          : [],
        demo: card.content.demo ? assets.videos[card.content.demo] : null,
        techArchitecture: Array.isArray(card.content.techArchitecture)
          ? card.content.techArchitecture.map(arch => assets.architectures[arch])
          : [],
        descriptionFlow: Array.isArray(card.content.description)
          ? card.content.description.map(desc => assets.descriptions[desc])
          : [],
        benefitsFlow: Array.isArray(card.content.benefits)
          ? card.content.benefits.map(benefit => assets.solutionsBenefits[benefit])
          : [],
        adoptionFlow: Array.isArray(card.content.adoption)
          ? card.content.adoption.map(adopt => assets.adoption[adopt])
          : [],
      },
    };
  }

  return [
    mapAssets(IntelligentAssist),
    // Map other cards similarly...
  ];
})()


// data.js

import CitizenAdvisor from './CardsData/CitizenAdvisor.json';
// Import other card data similarly...

import { fetchAssets } from './AssetImports';

export const cardsData = (async () => {
  const assets = await fetchAssets();
  if (!assets) return [];

  function mapAssets(card) {
    return {
      ...card,
      imageUrl: card.imageUrl ? assets.images[card.imageUrl] : null,
      content: {
        ...card.content,
        solutionFlow: Array.isArray(card.content.solutionFlow)
          ? card.content.solutionFlow.map(flow => assets.solutionFlows[flow])
          : [],
        demo: card.content.demo ? assets.videos[card.content.demo] : null,
        techArchitecture: Array.isArray(card.content.techArchitecture)
          ? card.content.techArchitecture.map(arch => assets.architectures[arch])
          : [],
        descriptionFlow: Array.isArray(card.content.description)
          ? card.content.description.map(desc => assets.descriptions[desc])
          : [],
        benefitsFlow: Array.isArray(card.content.benefits)
          ? card.content.benefits.map(benefit => assets.solutionsBenefits[benefit])
          : [],
        adoptionFlow: Array.isArray(card.content.adoption)
          ? card.content.adoption.map(adopt => assets.adoption[adopt])
          : [],
      },
    };
  }

  return [
    mapAssets(CitizenAdvisor),
    // Map other cards similarly...
  ];
})()




AssetImports.js:10 Failed to fetch urldata.json: 
TypeError: Failed to fetch
    at fetchUrldata (AssetImports.js:4:1)
    at fetchAssets (AssetImports.js:22:1)
    at data.js:9:1
    at ./src/data.js (data.js:42:1)
    at options.factory (react refresh:6:1)
    at __webpack_require__ (bootstrap:22:1)
    at fn (hot module replacement:61:1)
    at ./src/components/Sidebar/SideBarPage.js (SideBar.js:51:1)
    at options.factory (react refresh:6:1)
    at __webpack_require__ (bootstrap:22:1)
fetchUrldata	@	AssetImports.js:10
