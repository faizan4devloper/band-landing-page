import { images, videos, solutionFlows, architectures } from './AssetImports';
import descriptions from './Data/Descriptions.json';
import solutionFlowsData from './Data/SolutionsFlow.json';
import adoptionData from './Data/Adoption.json';

export const cardsData = [
  {
    imageUrl: images.IntelligentAss,
    title: "Intelligent Assist",
    description: "Efficiently search across an organization's document repository and retrieve relevant information to questions/requests from global workforces.",
    industry: "All",
    businessFunction: "Customer Support",
    content: {
      description: descriptions.intelligentAssist,
      solutionFlow: solutionFlows.IntelligentAssistFlow,
      solutionFlowText: solutionFlowsData.intelligentAssist,
      demo: videos.demoVideo3,
      techArchitecture: architectures.IntelligentAssistArchitecture,
      benefits: "The purpose of this solution is to enhance the customer experience, making it adaptable across various industries. Specifically, it helps customers better understand the products and services an organization offers. Below are some examples of how this solution can be implemented across different industries",      
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
      solutionFlow: solutionFlows.EmailEarFlow,
      solutionFlowText: solutionFlowsData.emailEAR,
      demo: videos.demoVideo1,
      techArchitecture: architectures.EmailEARArchitecture,
      benefits: "This solution assists organizations in enhancing the customer experience for email responses. Automating responses to customer queries or integrating with applications to retrieve data or generate tickets facilitates completing the overall process without delays.  Timely and accurate responses help in increased self-service, reduced customer service inquiries, and higher customer satisfaction.  Human review of generated responses allows validation of customer feedback and enhancement of the knowledge base via feedback-driven updates.  Overall agent productivity stands to improve, as the solution can be seen as an agent assist that enables reviewing automatically generated responses and interacting with customers accordingly.  The adaptability of the solution allows it to be implemented across various verticals to address common customer pain points.",     
      adoption: adoptionData.emailEAR
    }
  },
