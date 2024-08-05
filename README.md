// AssetImports.js
export const images = {
  IntelligentAssist: require('./components/Cards/CardsImages/card3.jpg'),
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

// Solution Flows
export const assets = require('https://aiml-convai.s3.amazonaws.com/portal-slides/URLJson.json');


// data.js
import IntelligentAssist from './CardsData/IntelligentAssist.json';
import EmailEAR from './CardsData/EmailEAR.json';
import CaseIntelligence from './CardsData/CaseIntelligence.json';
import SmartRecruit from './CardsData/SmartRecruit.json';
import IAssureClaim from './CardsData/IAssureClaim.json';
import AssistantEV from './CardsData/AssistantEV.json';
import AutoWiseCompanion from './CardsData/AutoWiseCompanion.json';
import CitizenAdvisor from './CardsData/CitizenAdvisor.json';
import FinCompetitor from './CardsData/FinCompetitor.json';
import SignatureExtraction from './CardsData/SignatureExtraction.json';
import AiForce from './CardsData/AiForce.json';
import ApiCase from './CardsData/ApiCase.json';
import AmsSupport from './CardsData/AmsSupport.json';
import CodeGreat from './CardsData/CodeGreat.json';
import AaigApi from './CardsData/AaigApi.json';
import ResponsibleGen from './CardsData/ResponsibleGen.json';
import GraphData from './CardsData/GraphData.json';
import PredictiveAsset from './CardsData/PredictiveAsset.json';

const { images, videos, assets } = require('./AssetImports');

function mapAssets(card, assetData) {
  const assetKey = card.title.replace(/\s+/g, '');
  const data = assetData[assetKey] || {};

  return {
    ...card,
    imageUrl: card.imageUrl ? images[card.imageUrl.split('.').pop()] : null,
    content: {
      ...card.content,
      solutionFlow: data.solutionFlow || [],
      demo: card.content.demo ? videos[card.content.demo.split('.').pop()] : null,
      techArchitecture: data.techArchitecture || [],
      descriptionFlow: data.description || [],
      benefitsFlow: data.benefits || [],
      adoption: data.adoption || []
    }
  };
}

export const cardsData = [
  mapAssets(IntelligentAssist, assets),
  mapAssets(EmailEAR, assets),
  mapAssets(CaseIntelligence, assets),
  mapAssets(SmartRecruit, assets),
  mapAssets(IAssureClaim, assets),
  mapAssets(AssistantEV, assets),
  mapAssets(AutoWiseCompanion, assets),
  mapAssets(CitizenAdvisor, assets),
  mapAssets(FinCompetitor, assets),
  mapAssets(SignatureExtraction, assets),
  mapAssets(AiForce, assets),
  mapAssets(ApiCase, assets),
  mapAssets(AmsSupport, assets),
  mapAssets(CodeGreat, assets),
  mapAssets(AaigApi, assets),
  mapAssets(ResponsibleGen, assets),
  mapAssets(GraphData, assets),
  mapAssets(PredictiveAsset, assets),
];

// CitizenAdvisor.json
{
  "imageUrl": "images.CitizenAdvisor",
  "title": "Citizen Advisor",
  "description": "An experience transformation from disconnected silos information to an intuitive, personalized revelations",
  "industry": "GOVT",
  "businessFunction": "Customer Experience",
  "content": {
    "description": ["citizenDescription"],
    "solutionFlow": ["solutionFlows.CitizenAdvisorFlow1", "solutionFlows.CitizenAdvisorFlow2", "solutionFlows.CitizenAdvisorFlow3", "solutionFlows.CitizenAdvisorFlow4"],
    "demo": "CitizenAdvisorDemo",
    "techArchitecture": ["CitizenAdvisorArchitecture"],
    "benefits": ["citizenBenefits"],
    "adoption": ["citizenAdoption"]
  }
}


// URLJson.json from s3 bucket
{
    "CitizenAdvisor": {
      "benefits": ["https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisorv1/benefits/Slide8.png"],
      "techArchitecture": ["https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisorv1/techArchitecture/Slide7.png"],
      "solutionFlow": [
        "https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisorv1/solutionFlow/Slide6.png",
        "https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisorv1/solutionFlow/Slide3.png",
        "https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisorv1/solutionFlow/Slide5.png",
        "https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisorv1/solutionFlow/Slide2.png",
        "https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisorv1/solutionFlow/Slide4.png"
      ],
      "description": ["https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisorv1/description/Slide1.png"],
      "adoption": ["https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisorv1/adoption/Slide9.png"]
    },
    "CaseIntelligence": {
      "description": ["https://aiml-convai.s3.amazonaws.com/portal-slides/caseintelligence/description/Slide1.png"],
      "techArchitecture": ["https://aiml-convai.s3.amazonaws.com/portal-slides/caseintelligence/techArchitecture/Slide3.png"],
      "adoption": ["https://aiml-convai.s3.amazonaws.com/portal-slides/caseintelligence/adoption/Slide5.png"],
      "solutionFlow": ["https://aiml-convai.s3.amazonaws.com/portal-slides/caseintelligence/solutionFlow/Slide2.png"],
      "benefits": ["https://aiml-convai.s3.amazonaws.com/portal-slides/caseintelligence/benefits/Slide4.png"]
    },
    "IntelligentAssist": {
      "description": ["https://aiml-convai.s3.amazonaws.com/portal-slides/intelligentassist/description/Slide1.png"],
      "techArchitecture": ["https://aiml-convai.s3.amazonaws.com/portal-slides/intelligentassist/techArchitecture/Slide3.png"],
      "adoption": ["https://aiml-convai.s3.amazonaws.com/portal-slides/intelligentassist/adoption/Slide5.png"],
      "solutionFlow": ["https://aiml-convai.s3.amazonaws.com/portal-slides/intelligentassist/solutionFlow/Slide2.png"],
      "benefits": ["https://aiml-convai.s3.amazonaws.com/portal-slides/intelligentassist/benefits/Slide4.png"]
    },
    "SmartRecruit": {
      "adoption": ["https://aiml-convai.s3.amazonaws.com/portal-slides/smartrecruit/adoption/Slide7.png"],
      "benefits": ["https://aiml-convai.s3.amazonaws.com/portal-slides/smartrecruit/benefits/Slide6.png"],
      "description": [
        "https://aiml-convai.s3.amazonaws.com/portal-slides/smartrecruit/description/Slide1.png",
        "https://aiml-convai.s3.amazonaws.com/portal-slides/smartrecruit/description/Slide2.png"
      ],
      "solutionFlow": [
        "https://aiml-convai.s3.amazonaws.com/portal-slides/smartrecruit/solutionFlow/Slide3.png",
        "https://aiml-convai.s3.amazonaws.com/portal-slides/smartrecruit/solutionFlow/Slide4.png"
      ],
      "techArchitecture": ["https://aiml-convai.s3.amazonaws.com/portal-slides/smartrecruit/techArchitecture/Slide5.png"]
    },
    "EmailEAR": {
      "description": ["https://aiml-convai.s3.amazonaws.com/portal-slides/emailear/description/Slide1.png"],
      "techArchitecture": ["https://aiml-convai.s3.amazonaws.com/portal-slides/emailear/techArchitecture/Slide3.png"],
      "adoption": ["https://aiml-convai.s3.amazonaws.com/portal-slides/emailear/adoption/Slide5.png"],
      "solutionFlow": ["https://aiml-convai.s3.amazonaws.com/portal-slides/emailear/solutionFlow/Slide2.png"],
      "benefits": ["https://aiml-convai.s3.amazonaws.com/portal-slides/emailear/benefits/Slide4.png"]
    }
  } the images and videos renders but assets not why
There are a few things going on here:

1. In your CitizenAdvisor.json file, you are referencing keys like "citizenDescription" that don't seem to exist in your URLJson.json file from S3. So those asset URLs won't be populated.

2. In mapAssets(), you are doing:

```js
data.solutionFlow || []
```

This will default to an empty array if the key doesn't exist in the assets JSON. So if the key is missing, you'll end up with an empty array rather than null.

3. In your cardsData array, some of the card objects like IntelligentAssist don't have a "content.demo" property defined. So the video URL will be null for those.

To fix:

1. Make sure the keys in your per-solution JSON files match the keys in the URLJson.json file.

2. Update mapAssets() to pass through null if the key is missing:

```js 
data[key] ?? null
```

3. Add the "demo" property to cards that need it.

This will ensure assets are null if missing, rather than empty arrays/values.

The assets should then render correctly if the keys match up! Let me know if any part is still unclear!
