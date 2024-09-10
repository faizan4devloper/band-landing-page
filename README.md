
import IntelligentAssist from './CardsData/IntelligentAssist.json';
import EmailEAR from './CardsData/EmailEAR.json';
import CaseIntelligence from './CardsData/CaseIntelligence.json';
import SmartRecruit from './CardsData/SmartRecruit.json';
import ClaimAssist from './CardsData/ClaimAssist.json';
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
import SopAssistance from './CardsData/SopAssistance.json';

import { images, fetchAssets } from './AssetImports';

// Mapping assets for a specific card
async function mapAssets(card) {
  const assets = await fetchAssets();
  
  // Sanitize the title to match the keys in urldata.json
  const assetKey = card.title.replace(/\s+/g, '');

  // Access the data for the specific asset
  const data = {
    solutionFlow: assets[`${assetKey}solutionFlow`] || null,
    techArchitecture: assets[`${assetKey}techArchitecture`] || null,
    description: assets[`${assetKey}description`] || null,
    benefits: assets[`${assetKey}benefits`] || null,
    adoption: assets[`${assetKey}adoption`] || null,
    demo: assets[`${assetKey}demo`] || null,
  };
  
  console.log(`Data for ${card.title}:`, data);

  return {
    ...card,
    imageUrl: images[card.imageUrl] || 'defaultImagePath.jpg', // Use a default image if the specified one is missing
    content: {
      ...card.content,
      solutionFlow: data.solutionFlow,
      demo: data.demo || null,
      techArchitecture: data.techArchitecture,
      description: data.description,
      benefits: data.benefits,
      adoption: data.adoption,
    },
  };
}

// Mapping cards data asynchronously
export async function getCardsData() {
  const cardsData = await Promise.all([
    mapAssets(IntelligentAssist),
    mapAssets(EmailEAR),
    mapAssets(CaseIntelligence),
    mapAssets(SmartRecruit),
    mapAssets(ClaimAssist),
    mapAssets(AssistantEV),
    mapAssets(AutoWiseCompanion),
    mapAssets(CitizenAdvisor),
    mapAssets(FinCompetitor),
    mapAssets(SignatureExtraction),
    mapAssets(AiForce),
    mapAssets(ApiCase),
    mapAssets(AmsSupport),
    mapAssets(CodeGreat),
    mapAssets(AaigApi),
    mapAssets(ResponsibleGen),
    mapAssets(GraphData),
    mapAssets(PredictiveAsset),
    mapAssets(SopAssistance),
  ]);

  return cardsData;
}


{"CitizenAdvisorbenefits": ["https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisorv1/benefits/Slide8.JPG"], "CitizenAdvisortechArchitecture": ["https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisorv1/techArchitecture/Slide7.JPG"], "CitizenAdvisorsolutionFlow": ["https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisorv1/solutionFlow/Slide6.JPG", "https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisorv1/solutionFlow/Slide3.JPG", "https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisorv1/solutionFlow/Slide5.JPG", "https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisorv1/solutionFlow/Slide2.JPG", "https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisorv1/solutionFlow/Slide4.JPG"], "CitizenAdvisordescription": ["https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisorv1/description/Slide1.JPG"], "CitizenAdvisoradoption": ["https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisorv1/adoption/Slide9.JPG"], "CaseIntelligencedescription": ["https://aiml-convai.s3.amazonaws.com/portal-slides/caseintelligence/description/Slide1.JPG"], "CaseIntelligencetechArchitecture": ["https://aiml-convai.s3.amazonaws.com/portal-slides/caseintelligence/techArchitecture/Slide3.JPG"], "CaseIntelligenceadoption": ["https://aiml-convai.s3.amazonaws.com/portal-slides/caseintelligence/adoption/Slide5.JPG"], "CaseIntelligencesolutionFlow": ["https://aiml-convai.s3.amazonaws.com/portal-slides/caseintelligence/solutionFlow/Slide2.JPG"], "CaseIntelligencebenefits": ["https://aiml-convai.s3.amazonaws.com/portal-slides/caseintelligence/benefits/Slide4.JPG"], "IntelligentAssistdescription": ["https://aiml-convai.s3.amazonaws.com/portal-slides/intelligentassist/description/Slide1.JPG"], "IntelligentAssisttechArchitecture": ["https://aiml-convai.s3.amazonaws.com/portal-slides/intelligentassist/techArchitecture/Slide3.JPG"], "IntelligentAssistadoption": ["https://aiml-convai.s3.amazonaws.com/portal-slides/intelligentassist/adoption/Slide5.JPG"], "IntelligentAssistsolutionFlow": ["https://aiml-convai.s3.amazonaws.com/portal-slides/intelligentassist/solutionFlow/Slide2.JPG"], "IntelligentAssistbenefits": ["https://aiml-convai.s3.amazonaws.com/portal-slides/intelligentassist/benefits/Slide4.JPG"], "SmartRecruitadoption": ["https://aiml-convai.s3.amazonaws.com/portal-slides/smartrecruit/adoption/Slide7.JPG"], "SmartRecruitbenefits": ["https://aiml-convai.s3.amazonaws.com/portal-slides/smartrecruit/benefits/Slide6.JPG"], "SmartRecruitdescription": ["https://aiml-convai.s3.amazonaws.com/portal-slides/smartrecruit/description/Slide1.JPG", "https://aiml-convai.s3.amazonaws.com/portal-slides/smartrecruit/description/Slide2.JPG"], "SmartRecruitsolutionFlow": ["https://aiml-convai.s3.amazonaws.com/portal-slides/smartrecruit/solutionFlow/Slide3.JPG", "https://aiml-convai.s3.amazonaws.com/portal-slides/smartrecruit/solutionFlow/Slide4.JPG"], "SmartRecruittechArchitecture": ["https://aiml-convai.s3.amazonaws.com/portal-slides/smartrecruit/techArchitecture/Slide5.JPG"], "EmailEardescription": ["https://aiml-convai.s3.amazonaws.com/portal-slides/emailear/description/Slide1.JPG"], "EmailEartechArchitecture": ["https://aiml-convai.s3.amazonaws.com/portal-slides/emailear/techArchitecture/Slide3.JPG"], "EmailEaradoption": ["https://aiml-convai.s3.amazonaws.com/portal-slides/emailear/adoption/Slide5.JPG"], "EmailEarsolutionFlow": ["https://aiml-convai.s3.amazonaws.com/portal-slides/emailear/solutionFlow/Slide2.JPG"], "EmailEarbenefits": ["https://aiml-convai.s3.amazonaws.com/portal-slides/emailear/benefits/Slide4.JPG"], "ClaimAssistadoption": ["https://aiml-convai.s3.amazonaws.com/portal-slides/claimassist/adoption/Slide7.JPG"], "ClaimAssistbenefits": ["https://aiml-convai.s3.amazonaws.com/portal-slides/claimassist/benefits/Slide6.JPG"], "ClaimAssistdescription": ["https://aiml-convai.s3.amazonaws.com/portal-slides/claimassist/description/Slide1.JPG"], "ClaimAssistsolutionFlow": ["https://aiml-convai.s3.amazonaws.com/portal-slides/claimassist/solutionFlow/Slide3.JPG", "https://aiml-convai.s3.amazonaws.com/portal-slides/claimassist/solutionFlow/Slide2.JPG", "https://aiml-convai.s3.amazonaws.com/portal-slides/claimassist/solutionFlow/Slide4.JPG"], "ClaimAssisttechArchitecture": ["https://aiml-convai.s3.amazonaws.com/portal-slides/claimassist/techArchitecture/Slide5.JPG"], "IFCdescription": ["https://aiml-convai.s3.amazonaws.com/portal-slides/ifc/description/Slide1.JPG"], "IFCtechArchitecture": ["https://aiml-convai.s3.amazonaws.com/portal-slides/ifc/techArchitecture/Slide3.JPG"], "IFCadoption": ["https://aiml-convai.s3.amazonaws.com/portal-slides/ifc/adoption/Slide5.JPG"], "IFCsolutionFlow": ["https://aiml-convai.s3.amazonaws.com/portal-slides/ifc/solutionFlow/Slide2.JPG"], "IFCbenefits": ["https://aiml-convai.s3.amazonaws.com/portal-slides/ifc/benefits/Slide4.JPG"], "AIForceadoption": ["https://aiml-convai.s3.amazonaws.com/portal-slides/aiforce/adoption/Slide7.JPG"], "AIForcebenefits": ["https://aiml-convai.s3.amazonaws.com/portal-slides/aiforce/benefits/Slide6.JPG"], "AIForcedescription": ["https://aiml-convai.s3.amazonaws.com/portal-slides/aiforce/description/Slide1.JPG"], "AIForcesolutionFlow": ["https://aiml-convai.s3.amazonaws.com/portal-slides/aiforce/solutionFlow/Slide3.JPG", "https://aiml-convai.s3.amazonaws.com/portal-slides/aiforce/solutionFlow/Slide2.JPG", "https://aiml-convai.s3.amazonaws.com/portal-slides/aiforce/solutionFlow/Slide4.JPG"], "AIForcetechArchitecture": ["https://aiml-convai.s3.amazonaws.com/portal-slides/aiforce/techArchitecture/Slide5.JPG"], "EVAssistantdescription": ["https://aiml-convai.s3.amazonaws.com/portal-slides/evassistant/description/Slide1.JPG"], "EVAssistanttechArchitecture": ["https://aiml-convai.s3.amazonaws.com/portal-slides/evassistant/techArchitecture/Slide3.JPG"], "EVAssistantadoption": ["https://aiml-convai.s3.amazonaws.com/portal-slides/evassistant/adoption/Slide5.JPG"], "EVAssistantsolutionFlow": ["https://aiml-convai.s3.amazonaws.com/portal-slides/evassistant/solutionFlow/Slide2.JPG"], "EVAssistantbenefits": ["https://aiml-convai.s3.amazonaws.com/portal-slides/evassistant/benefits/Slide4.JPG"], "AutoWiseadoption": ["https://aiml-convai.s3.amazonaws.com/portal-slides/autowise/adoption/Slide7.JPG"], "AutoWisebenefits": ["https://aiml-convai.s3.amazonaws.com/portal-slides/autowise/benefits/Slide6.JPG"], "AutoWisedescription": ["https://aiml-convai.s3.amazonaws.com/portal-slides/autowise/description/Slide1.JPG"], "AutoWisesolutionFlow": ["https://aiml-convai.s3.amazonaws.com/portal-slides/autowise/solutionFlow/Slide3.JPG", "https://aiml-convai.s3.amazonaws.com/portal-slides/autowise/solutionFlow/Slide2.JPG", "https://aiml-convai.s3.amazonaws.com/portal-slides/autowise/solutionFlow/Slide4.JPG"], "AutoWisetechArchitecture": ["https://aiml-convai.s3.amazonaws.com/portal-slides/autowise/techArchitecture/Slide5.JPG"], "CodeMentordescription": ["https://aiml-convai.s3.amazonaws.com/portal-slides/codementor/description/Slide1.JPG"], "CodeMentortechArchitecture": ["https://aiml-convai.s3.amazonaws.com/portal-slides/codementor/techArchitecture/Slide3.JPG"], "CodeMentoradoption": ["https://aiml-convai.s3.amazonaws.com/portal-slides/codementor/adoption/Slide5.JPG"], "CodeMentorsolutionFlow": ["https://aiml-convai.s3.amazonaws.com/portal-slides/codementor/solutionFlow/Slide2.JPG"], "CodeMentorbenefits": ["https://aiml-convai.s3.amazonaws.com/portal-slides/codementor/benefits/Slide4.JPG"], "SignatureSmartdescription": ["https://aiml-convai.s3.amazonaws.com/portal-slides/signaturesmart/description/Slide1.JPG"], "SignatureSmarttechArchitecture": ["https://aiml-convai.s3.amazonaws.com/portal-slides/signaturesmart/techArchitecture/Slide3.JPG"], "SignatureSmartadoption": ["https://aiml-convai.s3.amazonaws.com/portal-slides/signaturesmart/adoption/Slide5.JPG"], "SignatureSmartsolutionFlow": ["https://aiml-convai.s3.amazonaws.com/portal-slides/signaturesmart/solutionFlow/Slide2.JPG"], "SignatureSmartbenefits": ["https://aiml-convai.s3.amazonaws.com/portal-slides/signaturesmart/benefits/Slide4.JPG"], "CitizenAdvisordemo": ["https://aiml-convai.s3.amazonaws.com/portal-videos/CitizenAdvisorDemo.mp4"], "CaseIntelligencedemo": ["https://aiml-convai.s3.amazonaws.com/portal-videos/CaseIntelligenceDemo.mp4"], "IntelligentAssistdemo": ["https://aiml-convai.s3.amazonaws.com/portal-videos/IntelligentAssistDemo.mp4"], "SmartRecruitdemo": ["https://aiml-convai.s3.amazonaws.com/portal-videos/SmartRecruitDemo.mp4"], "EmailEardemo": ["https://aiml-convai.s3.amazonaws.com/portal-videos/EmailEARDemo.mp4"], "ClaimAssistdemo": ["https://aiml-convai.s3.amazonaws.com/portal-videos/ClaimAssistDemo.mp4"], "IFCdemo": ["https://aiml-convai.s3.amazonaws.com/portal-videos/IFCDemo.mp4"], "AIForcedemo": ["https://aiml-convai.s3.amazonaws.com/portal-videos/AIForceDemo.mp4"], "EVAssistantDemo": ["https://aiml-convai.s3.amazonaws.com/portal-videos/EVAssistantDemo.mp4"], "AutowiseDemo": ["https://aiml-convai.s3.amazonaws.com/portal-videos/Autowisedemo.mp4"], "CodeMentorDemo": ["https://aiml-convai.s3.amazonaws.com/portal-videos/CodeMentorDemo.mp4"], "SignatureSmartDemo": ["https://aiml-convai.s3.amazonaws.com/portal-videos/SignatureSmartDemo.mp4"]}



{
  "imageUrl": "FinanceCompetitor",
  "title": "IFC",
  "description": "Intelligent financial data analysis and insightful commentary generation using Gen AI.",
  "industry": "All",
  "businessFunction": "Finance"
}

