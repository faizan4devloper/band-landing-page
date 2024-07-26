{
  "intelligentAssist": [
    { "step": "Data Ingestion Pipeline", "description": "Automated data ingestion pipeline to process documents at scale." },
    { "step": "Document Text Extraction", "description": "Depending upon the document type (Audio, PDF, FAQ etc.), respective pipeline is triggered to extract text, tabular data from the documents." },
    { "step": "Data Chunking", "description": "To enable contextual storage and retrieval, large documents are split into smaller chunks." },
    { "step": "Vector Embedding Generation", "description": "All chunks are then processed to generate vector embeddings, which are stored in a vector database. This allows data querying and similarity-based searches using the vector embeddings." },
    { "step": "Natural Language Q&A", "description": "Interactive GUI enables users to ask questions related to the products/policies in a very natural language way." },
    { "step": "Contextual Response", "description": "The system checks relevant matching documents from the ingested knowledge repository. Based on the context, Gen AI model (LLM) generates the contextual response back to the user." },
    
  ],
  "emailEAR": [
    { "step": "Email Context Extraction", "description": "LLM powered Natural language processing is leveraged to extract the question or request from the email, classify the type of email, and understand the sentiment or tone of the sender." },
    { "step": "Agent Routing", "description": "Take advantage of LLM Agents to route the emails to the respective handler for subsequent actions." },
    { "step": "Actions", "description": "Perform the following actions based on the defined context." },
    { "step": "Response Generation", "description": "The system leverages LLM’natural language generation capabilities to compose a response incorporating the results of the actions above in a well-structured format tailored to the email context, classification, and sentiment." },
    { "step": "Explainability", "description": "The system can explain the reasoning and data flows behind its actions to enhance transparency using capabilities like the ReAct framework." },
  ]
  "smartRecruit": [
    { "step": "Screening Candidates", "description": "Reviewing resumes and screening candidates is tedious. Interview Assist uses Gen AI LLM’s natural language processing to quickly parse resumes and rank candidates based on relevance to the job description. This allows recruiters to focus on the most promising applicants." },
    { "step": "Generating Interview Questions", "description": "Coming up with relevant, thoughtful interview questions can be difficult. Interview Assist recommend personalized questions based on the candidate's background, resume and the Job Description." },
    { "step": "Summarizing Candidate Profiles", "description": "Interview Assist helps summarize the candidate’s work profile taking relevant experience, Job Description into consideration." },
    { "step": "Generating Interview Feedback", "description": "Providing quality feedback to candidates can be challenging. Interview Assist analyzes the candidate's resume and interview responses and automatically generates personalized feedback assessing strengths, weaknesses, and fit for the role. This saves time while providing helpful feedback to candidates." },
    { "step": "Facial Recognition", "description": " Interview Assist uses computer vision and facial recognition to identify candidates during video interviews. This automates attendance taking and ensures the intended candidate is present." },
  ]
}
