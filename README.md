export const fetchAssetUrls = async () => {
  const response = await fetch('https://aiml-convai.s3.amazonaws.com/urldata.json');
  const data = await response.json();

  const assetUrls = {};

  data.forEach(entry => {
    const [key, value] = Object.entries(entry)[0];
    assetUrls[key] = value;
  });

  return assetUrls;
};



import { fetchAssetUrls } from './utils/fetchAssetUrls';

let assetUrls = {};

const loadAssets = async () => {
  try {
    assetUrls = await fetchAssetUrls();
  } catch (error) {
    console.error('Error loading asset URLs:', error);
  }
};

loadAssets(); // Load the assets when the module is imported

export const images = {
  IntelligentAss: require('./components/Cards/CardsImages/card3.jpg'),
  EmailEAR: require('./components/Cards/CardsImages/card1.jpg'),
  CaseIntelligence: require('./components/Cards/CardsImages/card4.jpg'),
  SmartRecruit: require('./components/Cards/CardsImages/card8.jpg'),
  IAssureClaim: require('./components/Cards/CardsImages/card9.jpg'),
  AssistantEV: require('./components/Cards/CardsImages/card10.jpg'),
  CitizenAdvisor: require('./components/Cards/CardsImages/dummy2.jpg'),
  FinanceCompetitor: require('./components/Cards/CardsImages/card82.jpg'),
  Signature: require('./components/Cards/CardsImages/card2.jpg'),
  AIForce: require('./components/Cards/CardsImages/card19.jpg'),
  APICase: require('./components/Cards/CardsImages/card13.jpg'),
  AMSSupport: require('./components/Cards/CardsImages/AUTOMATION.jpg'),
  SOP: require('./components/Cards/CardsImages/SOP.jpg'),
  CodeGReat: require('./components/Cards/CardsImages/card5.jpg'),
  AAIG: require('./components/Cards/CardsImages/card16.jpg'),
  ResponsibleGen: require('./components/Cards/CardsImages/card17.jpg'),
  GraphData: require('./components/Cards/CardsImages/card18.jpg'),
  PredictiveAsset: require('./components/Cards/CardsImages/card11.jpg'),
};

// Videos
export const videos = {
  EmailEARDemo: 'https://aiml-convai.s3.amazonaws.com/demovideos/Email-EAR_Demo_new.mp4',
  SignatureExtractionDemo: 'https://aiml-convai.s3.amazonaws.com/demovideos/Sign_Verification_New.mp4',
  IntelligentAssistDemo: 'https://aiml-convai.s3.amazonaws.com/demovideos/Intelligent_Assist-QnA_DemoVideo_new.mp4',
  CaseIntelligenceDemo: 'https://aiml-convai.s3.amazonaws.com/demovideos/Case-Intelligence_demo.mp4',
  CodeGReatDemo: 'https://aiml-convai.s3.amazonaws.com/demovideos/CodeGreat_Demo_new.mp4',
  SmartRecruitDemo: 'https://aiml-convai.s3.amazonaws.com/demovideos/SmartRecruit_IvAssist_Demo.mp4',
  CitizenAdvisorDemo: 'https://aiml-convai.s3.amazonaws.com/demovideos/Citizen_Advisor-Demo1.mp4',
};

// Dynamic assets
export const dynamicAssets = {
  solutionFlows: assetUrls.CitizenAdvisorsolutionFlow || [],
  architectures: assetUrls.CitizenAdvisortechArchitecture || [],
  descriptions: assetUrls.CitizenAdvisordescription || [],
  solutionsBenefits: assetUrls.CitizenAdvisorbenefits || [],
  adoption: assetUrls.CitizenAdvisoradoption || [],
};




import CitizenAdvisor from './CardsData/CitizenAdvisor.json';

function mapAssets(card, dynamicAssets) {
  return {
    ...card,
    imageUrl: card.imageUrl ? images[card.imageUrl] : null,
    content: {
      ...card.content,
      solutionFlow: Array.isArray(card.content.solutionFlow)
        ? card.content.solutionFlow.map(flow => dynamicAssets.solutionFlows.find(url => url.includes(flow)))
        : (typeof card.content.solutionFlow === 'string' ? dynamicAssets.solutionFlows.find(url => url.includes(card.content.solutionFlow)) : []),
      demo: card.content.demo ? videos[card.content.demo] : null,
      techArchitecture: Array.isArray(card.content.techArchitecture)
        ? card.content.techArchitecture.map(arch => dynamicAssets.architectures.find(url => url.includes(arch)))
        : (typeof card.content.techArchitecture === 'string' ? dynamicAssets.architectures.find(url => url.includes(card.content.techArchitecture)) : []),
      description: Array.isArray(card.content.description)
        ? card.content.description.map(desc => dynamicAssets.descriptions.find(url => url.includes(desc)))
        : (typeof card.content.description === 'string' ? dynamicAssets.descriptions.find(url => url.includes(card.content.description)) : []),
      benefits: Array.isArray(card.content.benefits)
        ? card.content.benefits.map(benefit => dynamicAssets.solutionsBenefits.find(url => url.includes(benefit)))
        : (typeof card.content.benefits === 'string' ? dynamicAssets.solutionsBenefits.find(url => url.includes(card.content.benefits)) : []),
      adoption: Array.isArray(card.content.adoption)
        ? card.content.adoption.map(adopt => dynamicAssets.adoption.find(url => url.includes(adopt)))
        : (typeof card.content.adoption === 'string' ? dynamicAssets.adoption.find(url => url.includes(card.content.adoption)) : []),
    },
  };
}

export const cardsData = [
  mapAssets(CitizenAdvisor, dynamicAssets),
];

