import { images, videos, solutionFlows, architectures } from './AssetImports';



export const cardsData = [

{
    imageUrl: images.IntelligentAss,
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
  {
    imageUrl: images.EmailEAR,
    title: "Email EAR",
    description: "Transform customer support process by automating email analysis, and providing crafted thoughtful response to incoming emails",
    industry: "All",
    businessFunction: "Customer Support",
    content: {
      description: "Email EAR (Extract, Act and Respond) In today’s era, there are unprecedented ways of engaging the customer and this includes both traditional and digital channels. Unified experience across channels is key for customer delight.Despite the rise of alternative options, Email remains a preferred communication channel for customer support. Email provides unique benefits like maintaining a history of interactions, allowing customers to comprehensively explain issues, and enabling file attachments for additional context. However, the prioritization and queuing of emails often leads to delayed responses that create poor customer experiences. Our Gen AI-powered Email EAR (Extract, Act, and Respond) solution aims to transform the customer support process by automating the reading, analysis, and thoughtful responding to incoming emails. Specifically, our system can extract the core query, complaint, or issue within an email. It then understands what actions are required to resolve the customer's needs. Finally, it generates a user-friendly, detailed response explaining steps taken to address their questions or concerns.",
      solutionFlow: solutionFlows.solutionFlow1,
      solutionFlowText: [
        { step: "Email Context Extraction", description: "LLM powered Natural language processing is leveraged to extract the question or request from the email, classify the type of email, and understand the sentiment or tone of the sender." },
        { step: "Agent Routing", description: "Take advantage of LLM Agents to route the emails to the respective handler for subsequent actions." },
        { step: "Actions", description: "Perform the following actions based on the defined context." },
        { step: "Response Generation", description: "The system leverages LLM’natural language generation capabilities to compose a response incorporating the results of the actions above in a well-structured format tailored to the email context, classification, and sentiment." },
        { step: "Explainability", description: "The system can explain the reasoning and data flows behind its actions to enhance transparency using capabilities like the ReAct framework." },
      ],
      demo: videos.demoVideo1,
      techArchitecture: architectures.architecture1,
      benefits: "This solution assists organizations in enhancing the customer experience for email responses. Automating responses to customer queries or integrating with applications to retrieve data or generate tickets facilitates completing the overall process without delays.  Timely and accurate responses help in increased self-service, reduced customer service inquiries, and higher customer satisfaction.  Human review of generated responses allows validation of customer feedback and enhancement of the knowledge base via feedback-driven updates.  Overall agent productivity stands to improve, as the solution can be seen as an agent assist that enables reviewing automatically generated responses and interacting with customers accordingly.  The adaptability of the solution allows it to be implemented across various verticals to address common customer pain points.",
adoption: [
        { industry: "Financial", adoption: "Can help in answering finance product information, Loan eligibility, Product Recommendation as per the email context" },
        { industry: "Education", adoption: "Can help students to get response to their queries related to Admission process, Scholarship schemes, Enrollment process etc." },
        { industry: "Healthcare/Insurance", adoption: "Can help customers to understand their health insurance eligibility, claim processing, claim status etc." },
        { industry: "Retail", adoption: "Queries related to product exchange / return can be better handled" }
      ],      
    },
    
  },
