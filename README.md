{
  "intelligentAssist": "GenAI Intelligent Assist(Q&A) Every organization possesses a vast knowledge base embedded within its extensive document repositories...",
  "emailEAR": "Email EAR (Extract, Act and Respond) In today’s era, there are unprecedented ways of engaging the customer..."
}



{
  "intelligentAssist": [
    { "step": "Data Ingestion Pipeline", "description": "Automated data ingestion pipeline to process documents at scale." },
    { "step": "Document Text Extraction", "description": "Depending upon the document type (Audio, PDF, FAQ etc.), respective pipeline is triggered to extract text, tabular data from the documents." },
    ...
  ],
  "emailEAR": [
    { "step": "Email Context Extraction", "description": "LLM powered Natural language processing is leveraged to extract the question or request from the email, classify the type of email, and understand the sentiment or tone of the sender." },
    ...
  ]
}


{
  "intelligentAssist": [
    { "industry": "Financial", "adoption": "The solution could explain complex financial products and services to customers in straightforward language..." },
    ...
  ],
  "emailEAR": [
    { "industry": "Financial", "adoption": "Can help in answering finance product information, Loan eligibility, Product Recommendation as per the email context" },
    ...
  ]
}



import { images, videos, solutionFlows, architectures } from './AssetImports';
import descriptions from './data/descriptions.json';
import solutionFlowsData from './data/solutionFlows.json';
import adoptionData from './data/adoption.json';

export const cardsData = [
  {
    imageUrl: images.IntelligentAss,
    title: "Intelligent Assist",
    description: "Efficiently search across an organization's document repository and retrieve relevant information to questions/requests from global workforces.",
    industry: "All",
    businessFunction: "Customer Support",
    content: {
      description: descriptions.intelligentAssist,
      solutionFlow: solutionFlows.solutionFlow3,
      solutionFlowText: solutionFlowsData.intelligentAssist,
      demo: videos.demoVideo3,
      techArchitecture: architectures.architecture3,
      benefits: "The purpose of this solution is to enhance the customer experience, making it adaptable across various industries...",
      adoption: adoptionData.intelligentAssist
    }
  },
  {
    imageUrl: images.EmailEAR,
    title: "Email EAR",
    description: "Transform customer support process by automating email analysis, and providing crafted thoughtful response to incoming emails",
    industry: "All",
    businessFunction: "Customer Support",
    content: {
      description: descriptions.emailEAR,
      solutionFlow: solutionFlows.solutionFlow1,
      solutionFlowText: solutionFlowsData.emailEAR,
      demo: videos.demoVideo1,
      techArchitecture: architectures.architecture1,
      benefits: "This solution assists organizations in enhancing the customer experience for email responses...",
      adoption: adoptionData.emailEAR
    }
  }
];
