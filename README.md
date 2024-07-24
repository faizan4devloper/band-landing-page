// src/components/AssetImports.js

// Images
export const images = {
  IntelligentAss: require('../components/Cards/card3.jpg'),
  EmailEAR: require('../components/Cards/card1.jpg'),
  CaseIntelligence: require('../components/Cards/card4.jpg'),
  SmartRecruit: require('../components/Cards/card8.jpg'),
  IAssureClaim: require('../components/Cards/card9.jpg'),
  AssistantEV: require('../components/Cards/card10.jpg'),
  CitizenAdvisor: require('../components/Cards/dummy2.jpg'),
  FinanaceCompetitor: require('../components/Cards/card82.jpg'),
  Signature: require('../components/Cards/card2.jpg'),
  AIForce: require('../components/Cards/card19.jpg'),
  APICase: require('../components/Cards/card13.jpg'),
  AMSSupport: require('../components/Cards/AUTOMATION.jpg'),
  SOP: require('../components/Cards/SOP.jpg'),
  CodeGReat: require('../components/Cards/card5.jpg'),
  AAIG: require('../components/Cards/card16.jpg'),
  ResponsibleGen: require('../components/Cards/card17.jpg'),
  GraphData: require('../components/Cards/card18.jpg'),
  PredictiveAsset: require('../components/Cards/card11.jpg'),
};

// Videos
export const videos = {
  demoVideo1: require('../components/Sidebar/Icons/Email-EAR.mp4'),
  demoVideo2: require('../components/Sidebar/Icons/SignatureExtraction.mp4'),
  demoVideo3: require('../components/Sidebar/Icons/IntelligentAssist.mp4'),
  demoVideo4: require('../components/Sidebar/Icons/CaseIntelligent.mp4'),
  demoVideo5: require('../components/Sidebar/Icons/CodeGreat.mp4'),
  demoVideo6: require('../components/Sidebar/Icons/SmartRecruit.mp4'),
};

// Solution Flows
export const solutionFlows = {
  solutionFlow1: require('../components/Sidebar/Icons/solutionFlowGraph1.png'),
  solutionFlow3: require('../components/Sidebar/Icons/solutionFlowGraph3.png'),
  solutionFlow4: require('../components/Sidebar/Icons/solutionFlowGraph4.png'),
  solutionFlow6: require('../components/Sidebar/Icons/solutionFlowGraph6.png'),
};

// Technical Architectures
export const architectures = {
  architecture1: require('../components/Sidebar/Icons/technicalArchitecture1.png'),
  architecture3: require('../components/Sidebar/Icons/technicalArchitecture3.png'),
  architecture4: require('../components/Sidebar/Icons/technicalArchitecture4.png'),
  architecture5: require('../components/Sidebar/Icons/technicalArchitecture5.png'),
  architecture6: require('../components/Sidebar/Icons/technicalArchitecture6.png'),
};
// src/components/AssetImports.js

import { useMemo } from 'react';

// Images
const images = useMemo(() => ({
  IntelligentAss: require('../components/Cards/card3.jpg'),
  EmailEAR: require('../components/Cards/card1.jpg'),
  CaseIntelligence: require('../components/Cards/card4.jpg'),
  SmartRecruit: require('../components/Cards/card8.jpg'),
  IAssureClaim: require('../components/Cards/card9.jpg'),
  AssistantEV: require('../components/Cards/card10.jpg'),
  CitizenAdvisor: require('../components/Cards/dummy2.jpg'),
  FinanaceCompetitor: require('../components/Cards/card82.jpg'),
  Signature: require('../components/Cards/card2.jpg'),
  AIForce: require('../components/Cards/card19.jpg'),
  APICase: require('../components/Cards/card13.jpg'),
  AMSSupport: require('../components/Cards/AUTOMATION.jpg'),
  SOP: require('../components/Cards/SOP.jpg'),
  CodeGReat: require('../components/Cards/card5.jpg'),
  AAIG: require('../components/Cards/card16.jpg'),
  ResponsibleGen: require('../components/Cards/card17.jpg'),
  GraphData: require('../components/Cards/card18.jpg'),
  PredictiveAsset: require('../components/Cards/card11.jpg'),
}), []);

// Videos
const videos = useMemo(() => ({
  demoVideo1: require('../components/Sidebar/Icons/Email-EAR.mp4'),
  demoVideo2: require('../components/Sidebar/Icons/SignatureExtraction.mp4'),
  demoVideo3: require('../components/Sidebar/Icons/IntelligentAssist.mp4'),
  demoVideo4: require('../components/Sidebar/Icons/CaseIntelligent.mp4'),
  demoVideo5: require('../components/Sidebar/Icons/CodeGreat.mp4'),
  demoVideo6: require('../components/Sidebar/Icons/SmartRecruit.mp4'),
}), []);

// Solution Flows
const solutionFlows = useMemo(() => ({
  solutionFlow1: require('../components/Sidebar/Icons/solutionFlowGraph1.png'),
  solutionFlow3: require('../components/Sidebar/Icons/solutionFlowGraph3.png'),
  solutionFlow4: require('../components/Sidebar/Icons/solutionFlowGraph4.png'),
  solutionFlow6: require('../components/Sidebar/Icons/solutionFlowGraph6.png'),
}), []);

// Technical Architectures
const architectures = useMemo(() => ({
  architecture1: require('../components/Sidebar/Icons/technicalArchitecture1.png'),
  architecture3: require('../components/Sidebar/Icons/technicalArchitecture3.png'),
  architecture4: require('../components/Sidebar/Icons/technicalArchitecture4.png'),
  architecture5: require('../components/Sidebar/Icons/technicalArchitecture5.png'),
  architecture6: require('../components/Sidebar/Icons/technicalArchitecture6.png'),
}), []);

// Export all assets
export default {
  images,
  videos,
  solutionFlows,
  architectures,
};



// src/data.js
import AssetImports from './components/AssetImports';

const { images, videos, solutionFlows, architectures } = AssetImports;

export const cardsData = [
  {
    imageUrl: images.IntelligentAss,
    title: "Intelligent Assist",
    description: "Efficiently search across an organization's document repository...",
    industry: "All",
    businessFunction: "Customer Support",
    content: {
      description: "GenAI Intelligent Assist(Q&A) Every organization possesses a vast...",
      solutionFlow: solutionFlows.solutionFlow3,
      solutionFlowText: [
        { step: "Data Ingestion Pipeline", description: "Automated data ingestion pipeline to process documents at scale." },
        { step: "Document Text Extraction", description: "Depending upon the document type..." },
        // other steps...
      ],
      demo: videos.demoVideo3,
      techArchitecture: architectures.architecture3,
      benefits: "The purpose of this solution is to enhance the customer experience...",
      adoption: [
        { industry: "Financial", adoption: "The solution could explain complex financial products..." },
        // other industry adoptions...
      ]
    },
  },
  // Other cards...
];
