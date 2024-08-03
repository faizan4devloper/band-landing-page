// AssetImports.js
import urldata from './path/to/urldata.json'; // Adjust the path as necessary

// Function to get URLs from urldata.json
export function getAssetUrl(type, key) {
  const asset = urldata[type]?.find(item => item.key === key);
  return asset ? asset.url : null;
}

// Example usage
export const images = {
  CitizenAdvisor: getAssetUrl('images', 'CitizenAdvisor'),
  // Add other assets similarly...
};

export const videos = {
  CitizenAdvisorDemo: getAssetUrl('videos', 'CitizenAdvisorDemo'),
  // Add other assets similarly...
};

// Similarly update solutionFlows, architectures, descriptions, solutionsBenefits, adoption



import { getAssetUrl } from './AssetImports';

function mapAssets(card) {
  return {
    ...card,
    imageUrl: card.imageUrl ? getAssetUrl('images', card.imageUrl) : null,
    content: {
      ...card.content,
      solutionFlow: Array.isArray(card.content.solutionFlow)
        ? card.content.solutionFlow.map(flow => getAssetUrl('solutionFlows', flow))
        : [],
      demo: card.content.demo ? getAssetUrl('videos', card.content.demo) : null,
      techArchitecture: Array.isArray(card.content.techArchitecture)
        ? card.content.techArchitecture.map(arch => getAssetUrl('architectures', arch))
        : [],
      descriptionFlow: Array.isArray(card.content.description)
        ? card.content.description.map(desc => getAssetUrl('descriptions', desc))
        : [],
      benefitsFlow: Array.isArray(card.content.benefits)
        ? card.content.benefits.map(benefit => getAssetUrl('solutionsBenefits', benefit))
        : [],
      adoptionFlow: Array.isArray(card.content.adoption)
        ? card.content.adoption.map(adopt => getAssetUrl('adoption', adopt))
        : [],
    },
  };
}

export const cardsData = [
  // Import your JSON data and apply mapAssets
];



{
  "images": [
    {"key": "CitizenAdvisor", "url": "https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisor/description/CitizenAdvisorDescription.png"}
    // Add other images similarly...
  ],
  "videos": [
    {"key": "CitizenAdvisorDemo", "url": "https://aiml-convai.s3.amazonaws.com/demovideos/CitizenAdvisorDemo.mp4"}
    // Add other videos similarly...
  ],
  "solutionFlows": [
    {"key": "CitizenAdvisorFlow1", "url": "https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisor/solutionFlows/CitizenAdvisorFlow1.png"}
    // Add other solution flows similarly...
  ],
  "architectures": [
    {"key": "CitizenAdvisorArchitecture", "url": "https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisor/technical-architecture/CitizenAdvisorArchitecture.png"}
    // Add other architectures similarly...
  ],
  "descriptions": [
    {"key": "CitizenAdvisorDescription", "url": "https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisor/description/CitizenAdvisorDescription.png"}
    // Add other descriptions similarly...
  ],
  "solutionsBenefits": [
    {"key": "CitizenAdvisorBenefits", "url": "https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisor/benefits/CitizenAdvisorBenefits.png"}
    // Add other benefits similarly...
  ],
  "adoption": [
    {"key": "CitizenAdvisorAdoption", "url": "https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisor/industry-adoption/CitizenAdvisorAdoption.png"}
    // Add other adoption assets similarly...
  ]
}
