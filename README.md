{
  "imageUrl": "images.CitizenAdvisor",
  "title": "Citizen Advisor",
  "description": "An experience transformation from disconnected silos information to an intuitive, personalized revelations",
  "industry": "GOVT",
  "businessFunction": "Customer Experience",
  "content": {
    "descriptionFlow": "descriptions.DescriptionDemo",
    "solutionFlow": "solutionFlows.IntelligentAssistFlow",
    "demo": "videos.demoVideo5",
    "techArchitecture": "architectures.architecture5",
    "benefits": "Adoption: - By bringing together key capabilities for developers, IT ops, QA, and admins, our solution helps organizations deliver higher-quality software faster and more efficiently. It can help developers accelerate coding projects to boost productivity. DevOps engineers can create scripts to automate deployment pipelines for faster release cycles. Quality assurance specialists can use code analysis features to review code quality and adherence to best practices. Systems administrators can deploy monitoring and notification services to effectively manage infrastructure resources.",
    "adoption": [
      {"industry": "Financial", "adoption": "The solution could explain complex financial products and services to customers in straightforward language. Customers could receive personalized product recommendations based on their financial situations and goals."},
      {"industry": "Education", "adoption": "Can serve as virtual tutors or teaching assistants, answering students' questions, providing feedback, and adapting instruction to each learner's level of understanding. This enables more personalized and effective education for students."},
      {"industry": "Healthcare/Insurance", "adoption": "Can assist customers in understanding various insurance products, determining health insurance eligibility, and providing personalized insurance product recommendations tailored to each customer's needs."},
      {"industry": "HR", "adoption": "Can address employee inquiries regarding benefits, time off policies, training opportunities, and other related topics. This allows the company to provide 24/7 employee support and respond to questions in a timely manner."},
      {"industry": "Travel and Hospitality", "adoption": "The virtual assistant can recommend activities, respond to common travel questions, and otherwise assist with trip planning to improve the traveler's experience and convenience."},
      {"industry": "Retail", "adoption": "This solution could provide personalized recommendations and comparisons to help customers find the products best suited to their needs, answer common questions about product features and specifications."}
    ]
  }
}

function mapAssets(card) {
  return {
    ...card,
    imageUrl: card.imageUrl ? images[card.imageUrl.split('.').pop()] : null,
    content: {
      ...card.content,
      solutionFlow: card.content.solutionFlow ? solutionFlows[card.content.solutionFlow.split('.').pop()] : null,
      demo: card.content.demo ? videos[card.content.demo.split('.').pop()] : null,
      techArchitecture: card.content.techArchitecture ? architectures[card.content.techArchitecture.split('.').pop()] : null,
      descriptionFlow: card.content.description ? descriptions[card.content.description.split('.').pop()] : null,
    },
  };
}



export const images = {
  IntelligentAssist: require('./components/Cards/CardsImages/card3.jpg'),
  EmailEAR: require('./components/Cards/CardsImages/card1.jpg'),
  CaseIntelligence: require('./components/Cards/CardsImages/card4.jpg'),
  // ... other images
};

export const videos = {
  EmailEARDemo: 'https://aiml-convai.s3.amazonaws.com/demovideos/Email-EAR_Demo_new.mp4',
  SignatureExtractionDemo: 'https://aiml-convai.s3.amazonaws.com/demovideos/Sign_Verification_New.mp4',
  IntelligentAssistDemo: 'https://aiml-convai.s3.amazonaws.com/demovideos/Intelligent_Assist-QnA_DemoVideo_new.mp4',
  CaseIntelligenceDemo: 'https://aiml-convai.s3.amazonaws.com/demovideos/Case-Intelligence_demo.mp4',
  CodeGReatDemo: 'https://aiml-convai.s3.amazonaws.com/demovideos/CodeGreat_Demo_new.mp4',
  SmartRecruitDemo: 'https://aiml-convai.s3.amazonaws.com/demovideos/SmartRecruit_IvAssist_Demo.mp4',
};

export const solutionFlows = {
  EmailEarFlow: require('./components/Sidebar/Icons/EmailEarFlowGraph.png'),
  IntelligentAssistFlow: require('./components/Sidebar/Icons/IntelligentAssistFlowGraph.png'),
  CaseIntelligenceFlow: require('./components/Sidebar/Icons/CaseIntelligenceFlowGraph.png'),
  SmartRecruitFlow: require('./components/Sidebar/Icons/SmartRecruitFlowGraph.png'),
  IAssureClaimFlow: require('./components/Sidebar/Icons/IAssureClaimFlowGraph.png'),
};

export const architectures = {
  IntelligentAssistArchitecture: require('./components/Sidebar/Icons/IntelligentAssistarchitecture.png'),
  EmailEARArchitecture: require('./components/Sidebar/Icons/EmailEARarchitecture.png'),
  CaseIntelligenceArchitecture: require('./components/Sidebar/Icons/CaseIntelligencearchitecture.png'),
  SmartRecruitArchitecture: require('./components/Sidebar/Icons/SmartRecruitarchitecture.png'),
  AssistantEvArchitecture: require('./components/Sidebar/Icons/AssistantEvachitecture.png'),
  IAssureClaimArchitecture: require('./components/Sidebar/Icons/IAssureClaimarchitecture.png'),
  AIForceArchitecture: require('./components/Sidebar/Icons/AIForcearchitecture.png'),
};

export const descriptions = {
  DescriptionDemo: require('./components/Sidebar/Icons/CitizenAdvisorDesc.png'),
};
