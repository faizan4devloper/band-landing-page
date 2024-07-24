// src/data.js
import { images, videos, solutionFlows, architectures } from './components/AssetImports';

export const cardsData = [
  {
    imageUrl: images.IntelligentAssImg,
    title: "Intelligent Assist",
    description: "Efficiently search across an organization's document repository and retrieve relevant information to questions/requests from global workforces.",
    industry: "All",
    businessFunction: "Customer Support",
    content: {
      description: "GenAI Intelligent Assist(Q&A) Every organization possesses a vast knowledge base embedded within its extensive document repositories. This knowledge is critical for supporting both customers and internal employees in comprehending organizational processes, products, policies, and more. For large, multinational corporations with a diverse, multi-lingual workforce, Gen AI-powered intelligent assistants can serve as highly effective support agents by accurately retrieving precise information from these knowledge sources at scale. Our Gen AI Intelligent Assist leverages generative intelligence to efficiently search across an organization's documents and provide accurate, relevant information to questions or requests. With multi-lingual capabilities, it can serve global workforces by delivering the right knowledge to the right people when they need it, regardless of language or location.",
      solutionFlow: solutionFlows.solutionFlow3,
      solutionFlowText: [
        { step: "Data Ingestion Pipeline", description: "Automated data ingestion pipeline to process documents at scale." },
        { step: "Document Text Extraction", description: "Depending upon the document type (Audio, PDF, FAQ etc.), respective pipeline is triggered to extract text, tabular data from the documents." },
        { step: "Data Chunking", description: "To enable contextual storage and retrieval, large documents are split into smaller chunks." },
        { step: "Vector Embedding Generation", description: "All chunks are then processed to generate vector embeddings, which are stored in a vector database. This allows data querying and similarity-based searches using the vector embeddings." },
        { step: "Natural Language Q&A", description: "Interactive GUI enables users to ask questions related to the products/policies in a very natural language way." },
        { step: "Contextual Response", description: "The system checks relevant matching documents from the ingested knowledge repository. Based on the context, Gen AI model (LLM) generates the contextual response back to the user." },
      ],
      demo: videos.demoVideo3,
      techArchitecture: architectures.architecture3,
      benefits: "The purpose of this solution is to enhance the customer experience, making it adaptable across various industries. Specifically, it helps customers better understand the products and services an organization offers. Below are some examples of how this solution can be implemented across different industries",
      adoption: [
        { industry: "Financial", adoption: "The solution could explain complex financial products and services to customers in straightforward language. Customers could receive personalized product recommendations based on their financial situations and goals." },
        { industry: "Education", adoption: "Can serve as virtual tutors or teaching assistants, answering students' questions, providing feedback, and adapting instruction to each learner's level of understanding. This enables more personalized and effective education for students." },
        { industry: "Healthcare/Insurance", adoption: "Can assist customers in understanding various insurance products, determining health insurance eligibility, and providing personalized insurance product recommendations tailored to each customer's needs." },
        { industry: "HR", adoption: "Can address employee inquiries regarding benefits, time off policies, training opportunities, and other related topics. This allows the company to provide 24/7 employee support and respond to questions in a timely manner." },
        { industry: "Travel and Hospitality", adoption: "The virtual assistant can recommend activities, respond to common travel questions, and otherwise assist with trip planning to improve the traveler's experience and convenience." },
        { industry: "Retail", adoption: "This solution could provide personalized recommendations and comparisons to help customers find the products best suited to their needs, answer common questions about product features and specifications." },
      ]
    },
  },
  // Other cards...
];

import AssetImports from './AssetImports';

const { images, videos, solutionFlows, architectures } = AssetImports;


export const cardsData = [

{
    imageUrl: images.IntelligentAssImg,
    title: "Intelligent Assist",
    description: "Efficiently search across an organization's document repository and retrieve relevant information to questions/requests from global workforces.",
    industry: "All",
    businessFunction: "Customer Support",
    content: {
      description: "GenAI Intelligent Assist(Q&A) Every organization possesses a vast knowledge base embedded within its extensive document repositories. This knowledge is critical for supporting both customers and internal employees in comprehending organizational processes, products, policies, and more. For large, multinational corporations with a diverse, multi-lingual workforce, Gen AI-powered intelligent assistants can serve as highly effective support agents by accurately retrieving precise information from these knowledge sources at scale. Our Gen AI Intelligent Assist leverages generative intelligence to efficiently search across an organization's documents and provide accurate, relevant information to questions or requests. With multi-lingual capabilities, it can serve global workforces by delivering the right knowledge to the right people when they need it, regardless of language or location.",
      solutionFlow: solutionFlows.solutionFlow3,
      solutionFlowText: [
        { step: "Data Ingestion Pipeline", description: "Automated data ingestion pipeline to process documents at scale." },
        { step: "Document Text Extraction", description: "Depending upon the document type (Audio, PDF, FAQ etc.), respective pipeline is triggered to extract text, tabular data from the documents." },
        { step: "Data Chunking", description: "To enable contextual storage and retrieval, large documents are split into smaller chunks." },
        { step: "Vector Embedding Generation", description: "All chunks are then processed to generate vector embeddings, which are stored in a vector database. This allows data querying and similarity-based searches using the vector embeddings." },
        { step: "Natural Language Q&A", description: "Interactive GUI enables users to ask questions related to the products/policies in a very natural language way." },
        { step: "Contextual Response", description: "The system checks relevant matching documents from the ingested knowledge repository. Based on the context, Gen AI model (LLM) generates the contextual response back to the user." },
      ],
      demo: videos.demoVideo3,
      techArchitecture: architectures.architecture3,
      benefits: "The purpose of this solution is to enhance the customer experience, making it adaptable across various industries. Specifically, it helps customers better understand the products and services an organization offers. Below are some examples of how this solution can be implemented across different industries",
      adoption: [
        { industry: "Financial", adoption: "The solution could explain complex financial products and services to customers in straightforward language. Customers could receive personalized product recommendations based on their financial situations and goals." },
        { industry: "Education", adoption: "Can serve as virtual tutors or teaching assistants, answering students' questions, providing feedback, and adapting instruction to each learner's level of understanding. This enables more personalized and effective education for students." },
        { industry: "Healthcare/Insurance", adoption: "Can assist customers in understanding various insurance products, determining health insurance eligibility, and providing personalized insurance product recommendations tailored to each customer's needs." },
        { industry: "HR", adoption: "Can address employee inquiries regarding benefits, time off policies, training opportunities, and other related topics. This allows the company to provide 24/7 employee support and respond to questions in a timely manner." },
        { industry: "Travel and Hospitality", adoption: "The virtual assistant can recommend activities, respond to common travel questions, and otherwise assist with trip planning to improve the traveler's experience and convenience." },
        { industry: "Retail", adoption: "This solution could provide personalized recommendations and comparisons to help customers find the products best suited to their needs, answer common questions about product features and specifications." },
      ]
    },
  },


  export 'default' (imported as 'AssetImports') was not found in './AssetImports' (possible exports: architectures, images, solutionFlows, videos)
