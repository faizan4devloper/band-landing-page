{
  "images": [
    {"key": "CitizenAdvisor", "url": "https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisor/description/CitizenAdvisorDescription.png"},
    {"key": "SmartRecruit", "url": "https://aiml-convai.s3.amazonaws.com/portal-slides/smart-recruit/description/SmartRecruitDescription1.png"},
    {"key": "EmailEAR", "url": "https://aiml-convai.s3.amazonaws.com/portal-slides/emailear/description/Slide1.JPG"}
  ],
  "videos": [
    {"key": "CitizenAdvisorDemo", "url": "https://aiml-convai.s3.amazonaws.com/demovideos/Citizen_Advisor-Demo1.mp4"},
    {"key": "SmartRecruitDemo", "url": "https://aiml-convai.s3.amazonaws.com/demovideos/SmartRecruit_IvAssist_Demo.mp4"},
    {"key": "EmailEARDemo", "url": "https://aiml-convai.s3.amazonaws.com/demovideos/Email-EAR_Demo_new.mp4"}
  ],
  "solutionFlows": [
    {"key": "CitizenAdvisorFlow1", "url": "https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisor/solutionFlows/CitizenAdvisorFlow1.png"},
    {"key": "SmartRecruitFlow1", "url": "https://aiml-convai.s3.amazonaws.com/portal-slides/smart-recruit/solutionFlows/SmartRecruitSolutionFlow1.png"},
    {"key": "EmailEarFlow", "url": "https://aiml-convai.s3.amazonaws.com/portal-slides/emailear/solutionFlow/Slide2.JPG"}
  ],
  "architectures": [
    {"key": "CitizenAdvisorArchitecture", "url": "https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisor/technical-architecture/CitizenAdvisorArchitecture.png"},
    {"key": "SmartRecruitArchitecture", "url": "https://aiml-convai.s3.amazonaws.com/portal-slides/smart-recruit/technical-architecture/SmartRecruitArchitecture.png"},
    {"key": "EmailEARArchitecture", "url": "https://aiml-convai.s3.amazonaws.com/portal-slides/emailear/techArchitecture/Slide3.JPG"}
  ],
  "descriptions": [
    {"key": "CitizenAdvisorDescription", "url": "https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisor/description/CitizenAdvisorDescription.png"},
    {"key": "SmartRecruitDescription1", "url": "https://aiml-convai.s3.amazonaws.com/portal-slides/smart-recruit/description/SmartRecruitDescription1.png"},
    {"key": "EmailEARDemo", "url": "https://aiml-convai.s3.amazonaws.com/portal-slides/emailear/description/Slide1.JPG"}
  ],
  "solutionsBenefits": [
    {"key": "CitizenAdvisorBenefits", "url": "https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisor/benefits/CitizenAdvisorBenefits.png"},
    {"key": "SmartRecruitBenefits", "url": "https://aiml-convai.s3.amazonaws.com/portal-slides/smart-recruit/benefits/SmartRecruitBenefits.png"},
    {"key": "EmailEARBenefits", "url": "https://aiml-convai.s3.amazonaws.com/portal-slides/emailear/benefits/Slide4.JPG"}
  ],
  "adoption": [
    {"key": "CitizenAdvisorAdoption", "url": "https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisor/industry-adoption/CitizenAdvisorAdoption.png"},
    {"key": "SmartRecruitAdoption", "url": "https://aiml-convai.s3.amazonaws.com/portal-slides/smart-recruit/industry-adoption/SmartRecruitAdoption.png"},
    {"key": "EmailEARAdoption", "url": "https://aiml-convai.s3.amazonaws.com/portal-slides/emailear/adoption/Slide5.JPG"}
  ]
}







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
