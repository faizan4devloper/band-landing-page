
{
  "imageUrl": "images.CitizenAdvisor",
  "title": "Citizen Advisor",
  "description": "An experience transformation from disconnected silos information to an intuitive, personalized revelations",
  "industry": "GOVT",
  "businessFunction": "Customer Experience",
  "content": {
    "description": "descriptions.DescriptionDemo",
    "solutionFlow": ["solutionFlows.IntelligentAssistFlow1", "solutionFlows.IntelligentAssistFlow2", "solutionFlows.IntelligentAssistFlow3", "solutionFlows.IntelligentAssistFlow4"],
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




const images = {
  CitizenAdvisor: 'path/to/citizenAdvisorImage.jpg',
  IntelligentAssistFlow1: 'path/to/intelligentAssistFlow1.jpg',
  IntelligentAssistFlow2: 'path/to/intelligentAssistFlow2.jpg',
  IntelligentAssistFlow3: 'path/to/intelligentAssistFlow3.jpg',
  IntelligentAssistFlow4: 'path/to/intelligentAssistFlow4.jpg',
  // Add other images similarly
};

const videos = {
  demoVideo5: 'path/to/demoVideo5.mp4',
  // Add other videos similarly
};

const solutionFlows = {
  IntelligentAssistFlow1: 'path/to/intelligentAssistFlow1.jpg',
  IntelligentAssistFlow2: 'path/to/intelligentAssistFlow2.jpg',
  IntelligentAssistFlow3: 'path/to/intelligentAssistFlow3.jpg',
  IntelligentAssistFlow4: 'path/to/intelligentAssistFlow4.jpg',
  // Add other solution flows similarly
};

const architectures = {
  architecture5: 'path/to/architecture5.jpg',
  // Add other architectures similarly
};

const descriptions = {
  DescriptionDemo: 'path/to/descriptionDemo.jpg',
  // Add other descriptions similarly
};

module.exports = { images, videos, solutionFlows, architectures, descriptions };




function mapAssets(card) {
  return {
    ...card,
    imageUrl: card.imageUrl ? images[card.imageUrl.split('.').pop()] : null,
    content: {
      ...card.content,
      solutionFlow: card.content.solutionFlow ? card.content.solutionFlow.map(flow => solutionFlows[flow.split('.').pop()]) : [],
      demo: card.content.demo ? videos[card.content.demo.split('.').pop()] : null,
      techArchitecture: card.content.techArchitecture ? architectures[card.content.techArchitecture.split('.').pop()] : null,
      descriptionFlow: card.content.description ? descriptions[card.content.description.split('.').pop()] : null,
    },
  };
}

export const cardsData = [
  mapAssets(IntelligentAssist),
  mapAssets(EmailEAR),
  mapAssets(CaseIntelligence),
  mapAssets(SmartRecruit),
  mapAssets(IAssureClaim),
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
  // Map other card JSON files similarly
];


.mainContent {
  /* your existing styles */
}

.description {
  /* your existing styles */
}

.solution {
  /* your existing styles */
}

.demo {
  /* your existing styles */
}

.architecture {
  /* your existing styles */
}

.benefits {
  /* your existing styles */
}

.adoption {
  /* your existing styles */
}

.highlight {
  background-color: yellow; /* or any highlight color */
}

.maximized {
  width: 100%;
  height: auto;
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  z-index: 1000;
}

.overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.8);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 999;
}

.closeIcon {
  position: absolute;
  top: 20px;
  right: 20px;
  cursor: pointer;
  color: white;
  font-size: 24px;
  z-index: 1001;
}
