import image1 from "./components/Cards/card1.jpg";
import image2 from "./components/Cards/card2.jpg";
import image3 from "./components/Cards/card3.jpg";
import image4 from "./components/Cards/card4.jpg";
import image5 from "./components/Cards/card5.jpg";

import demoVideo1 from "./components/Sidebar/Icons/Email-EAR.mp4";
import demoVideo2 from "./components/Sidebar/Icons/SignatureExtraction.mp4";
import demoVideo3 from "./components/Sidebar/Icons/IntelligentAssist.mp4";
import demoVideo4 from "./components/Sidebar/Icons/CaseIntelligent.mp4";
import demoVideo5 from "./components/Sidebar/Icons/CodeGreat.mp4";

import solutionFlow1 from "./components/Sidebar/Icons/solutionFlowGraph1.png";
// import solutionFlow2 from "./components/Sidebar/Icons/solutionFlowGraph2.png";
import solutionFlow3 from "./components/Sidebar/Icons/solutionFlowGraph3.png";
import solutionFlow4 from "./components/Sidebar/Icons/solutionFlowGraph4.png";
// import solutionFlow4 from "./components/Icons/solutionFlow4.png";
// import solutionFlow5 from "./components/Icons/solutionFlow5.png";

import architecture1 from "./components/Sidebar/Icons/technicalArchitecture1.png";
import architecture2 from "./components/Sidebar/Icons/technicalArchitecture2.png";
import architecture3 from "./components/Sidebar/Icons/technicalArchitecture3.png";
import architecture4 from "./components/Sidebar/Icons/technicalArchitecture4.png";
import architecture5 from "./components/Sidebar/Icons/technicalArchitecture5.png";

export const cardsData = [
  {
    imageUrl: image1,
    title: "Email EAR",
    description: "Transform customer support process by automating email analysis, and providing crafted thoughtful response to incoming emails",
    content: {
      description: "Email EAR (Extract, Act and Respond) In today’s era, there are unprecedented ways of engaging the customer and this includes both traditional and digital channels. Unified experience across channels is key for customer delight.Despite the rise of alternative options, Email remains a preferred communication channel for customer support. Email provides unique benefits like maintaining a history of interactions, allowing customers to comprehensively explain issues, and enabling file attachments for additional context. However, the prioritization and queuing of emails often leads to delayed responses that create poor customer experiences. Our Gen AI-powered Email EAR (Extract, Act, and Respond) solution aims to transform the customer support process by automating the reading, analysis, and thoughtful responding to incoming emails. Specifically, our system can extract the core query, complaint, or issue within an email. It then understands what actions are required to resolve the customer's needs. Finally, it generates a user-friendly, detailed response explaining steps taken to address their questions or concerns.",
      solutionFlow: solutionFlow1,
      demo: demoVideo1,
      techArchitecture: architecture1,
      benefits: "This solution assists organizations in enhancing the customer experience for email responses. Automating responses to customer queries or integrating with applications to retrieve data or generate tickets facilitates completing the overall process without delays.  Timely and accurate responses help in increased self-service, reduced customer service inquiries, and higher customer satisfaction.  Human review of generated responses allows validation of customer feedback and enhancement of the knowledge base via feedback-driven updates.  Overall agent productivity stands to improve, as the solution can be seen as an agent assist that enables reviewing automatically generated responses and interacting with customers accordingly.  The adaptability of the solution allows it to be implemented across various verticals to address common customer pain points.",
 adoption: [
        { industry: "Financial", adoption: "Can help in answering finance product information, Loan eligibility, Product Recommendation as per the email context" },
        { industry: "Education", adoption: "Can help students to get response to their queries related to Admission process, Scholarship schemes, Enrollment process etc." },
        { industry: "Healthcare/Insurance", adoption: "Can help customers to understand their health insurance eligibility, claim processing, claim status etc." },
        { industry: "Retail", adoption: "Queries related to product exchange / return can be better handled" }
      ],      
    },
    
  },
  {
    imageUrl: image2,
    title: "Signature Extraction & Verification",
    description: "Automated Signature Extraction and Verification leveraging Amazon Textract and Siamese Network deployed on Amazon SageMaker.",
    content: {
      description: "Signature Extraction & Verification Signatures are unique to each person and can be used to verify identity and authorize documents. Extracting signatures from documents and verifying their authenticity is crucial for security, preventing forgery and ensuring validity of legal agreements. AI/ML algorithms can analyze shape, stroke style and pressure to accurately match signatures. Automating extraction and verification improves efficiency and allows large volumes of documents to be quickly processed. Below is the expected outcome from the solution:- The proposed system will have the ability to search through scanned or faxed multi-page documents to identify and extract any handwritten signatures present.  Once a signature is extracted, it can be compared against reference signatures stored in an Imaging Signature Verification database to determine if the extracted signature matches or mismatches a known signature.",
      solutionFlow: "Signature detection and Extraction: The system will be able to process scanned or faxed documents of multiple pages. It will search through the document images to identify signatures that appear to be handwritten. These identified signatures will then be extracted from the larger document image as separate signature image files. As signatures may appear on different pages and in different locations within the documents, the system will have robust capabilities to detect and extract these signatures regardless of placement. Signature Matching/Verification: The extracted signature images will be compared against known specimen signatures stored in the Imaging Signature Verification database or uploaded during the process. Deep Learning CNN based Siamese Network will be trained and deployed to determine if the extracted signature sufficiently matches a reference signature to consider it verified. A match percentage score can be generated to indicate the level of similarity.",
      demo: demoVideo2,
      techArchitecture:architecture2,
      benefits: "The purpose of this solution is to enhance the customer experience, making it adaptable across various industries. Specifically, it helps customers better understand the products and services an organization offers. Below are some examples of how this solution can be implemented across different industries",
      adoption: [
             { industry: "Financial", adoption: "Verifying signatures on cheques, account opening documents, etc. This can help prevent fraud and ensure identities are properly verified." },
             { industry: "Goverment/Public Sector", adoption: "Verifying signatures on tax forms, ballot initiatives, licenses, etc. This ensures proper authorization and prevents forgeries." },
             { industry: "Healthcare", adoption: "Verifying signatures on prescriptions, patient intake forms, insurance documents. Ensures valid authorizations and prevents fraud." },
             { industry: "HR", adoption: "Verifying signatures on employment agreements, benefit forms, etc. Confirms identity and consent." },
             { industry: "Insurance", adoption: "Verifying signatures on claims, underwriting applications, etc. Helps prevent fraud and ensure valid submissions." },
        ]
    },
  },
  {
    imageUrl: image3,
    title: "Intelligent Assist",
    description: "Efficiently search across an organization's document repository and retrieve relevant information to questions/requests from global workforces.",
    content: {
      description: "GenAI Intelligent Assist(Q&A) Every organization possesses a vast knowledge base embedded within its extensive document repositories. This knowledge is critical for supporting both customers and internal employees in comprehending organizational processes, products, policies, and more. For large, multinational corporations with a diverse, multi-lingual workforce, Gen AI-powered intelligent assistants can serve as highly effective support agents by accurately retrieving precise information from these knowledge sources at scale. Our Gen AI Intelligent Assist leverages generative intelligence to efficiently search across an organization's documents and provide accurate, relevant information to questions or requests. With multi-lingual capabilities, it can serve global workforces by delivering the right knowledge to the right people when they need it, regardless of language or location.",
      solutionFlow: solutionFlow3,
      demo: demoVideo3,
      techArchitecture: architecture3,
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
    imageUrl: image4,
    title: "Case Intelligence",
    description: "Automate case intake from various channels like email, handwritten forms... streamlining message intake process.",
    content: {
      description: "GenAI Case Intelligence Pharmaceutical companies around the world typically offer patient support services where patients can report medication side effects, usage, and symptoms. Patients provide this information through phone calls, emails, written forms, and other manual channels. Managing these manual intake processes can be time-consuming and may lead to missed key details during information exchange. Our Gen AI-powered Case Intelligence solution automates case intake from various channels like email, call transcripts, and handwritten forms. This streamlines the process and ensures no critical information is overlooked, regardless of how patients submit their reports.",
      solutionFlow: solutionFlow4,
      demo: demoVideo4,
      techArchitecture: architecture4,
      benefits: "Industry Adoption:- Although originally designed for the life sciences and healthcare industries, the approach and framework presented in this solution can be readily customized and adopted across various industries.",
       adoption: [
             { industry: "Financial", adoption: "Extracting key details such as names, addresses, account numbers, etc. from documents including loan applications, account opening forms, and insurance claims. Consolidating this information into unified customer profiles." },
             { industry: "Marketing", adoption: "Extracting names, contact information, preferences from forms including surveys, contest entries, and mailing list sign-ups. Compiling this data into customer profiles for marketing automation purposes." },
             { industry: "Legal", adoption: "Extracting key details from legal documents including contracts, agreements, and filings for due diligence and analysis. Summarizing contract terms for tracking and reporting." },
             { industry: "HR", adoption: "Extracting information including names, education, skills, and experience from resumes and employment forms. Compiling candidate profiles for recruiting and hiring purposes." },
             { industry: "Travel and Hospitality", adoption: "The virtual assistant can recommend activities, respond to common travel questions, and otherwise assist with trip planning to improve the traveler's experience and convenience." },
             { industry: "Customer Service (Across)", adoption: "xtracting customer information, product/service details, and complaint summaries from customer service forms and records. Consolidating into comprehensive customer case profiles." },
        ]
    },
  },
  {
    imageUrl: image5,
    title: "Code GReat",
    description: "Code assistant streamlining code development through NL conversations for generation, review, and execution.",
    content: {
      description: "Code GReat (Generate, Review & Execute Code) The use of generative AI to assist with code development and deployment is garnering interest across organizations. Integrated development environment (IDE) tools like Amazon Code Whisperer and Microsoft GitHub Copilot that leverage large language models are being evaluated and utilized to accelerate development work. Our Code GReat solution utilizes a large language model-enabled graphical user interface to facilitate natural language interactions for code generation, review, and execution. Users can generate programming code, review it, and execute it in a conversational way. Code GReat aims to streamline the development process leveraging the power of generative AI.",
      solutionFlow: " The system can translate natural language prompts into source code across a variety of programming languages and infrastructure as code (IAC). It can perform code reviews and code hygiene checks to identify potential issues.  The system provides the option to leverage different large language models (LLMs) via integration with AWS services like Bedrock and SageMaker Jumpstart.  It can generate code for many scenarios, such as 'Generate a Terraform template to deploy an AWS EC2 instance with Load Balancer' or 'Bash command to find all files created by userA in Linux.  The system has the potential to be extended further to execute the generated code automatically, enabling full end-to-end infrastructure provisioning and configuration from natural language prompts.",
      demo: demoVideo5,
      techArchitecture: architecture5,
      benefits: "Adoption: - By bringing together key capabilities for developers, IT ops, QA, and admins, our solution helps organizations deliver higher-quality software faster and more efficiently. It can help developers accelerate coding projects to boost productivity. DevOps engineers can create scripts to automate deployment pipelines for faster release cycles. Quality assurance specialists can use code analysis features to review code quality and adherence to best practices. Systems administrators can deploy monitoring and notification services to effectively manage infrastructure resources.",
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
    imageUrl: image5,
    title: "UK ADVISOR",
    description: "Code assistant streamlining code development through NL conversations for generation, review, and execution.",
    content: {
      description: "Code GReat (Generate, Review & Execute Code) The use of generative AI to assist with code development and deployment is garnering interest across organizations. Integrated development environment (IDE) tools like Amazon Code Whisperer and Microsoft GitHub Copilot that leverage large language models are being evaluated and utilized to accelerate development work. Our Code GReat solution utilizes a large language model-enabled graphical user interface to facilitate natural language interactions for code generation, review, and execution. Users can generate programming code, review it, and execute it in a conversational way. Code GReat aims to streamline the development process leveraging the power of generative AI.",
      solutionFlow: " The system can translate natural language prompts into source code across a variety of programming languages and infrastructure as code (IAC). It can perform code reviews and code hygiene checks to identify potential issues.  The system provides the option to leverage different large language models (LLMs) via integration with AWS services like Bedrock and SageMaker Jumpstart.  It can generate code for many scenarios, such as 'Generate a Terraform template to deploy an AWS EC2 instance with Load Balancer' or 'Bash command to find all files created by userA in Linux.  The system has the potential to be extended further to execute the generated code automatically, enabling full end-to-end infrastructure provisioning and configuration from natural language prompts.",
      demo: demoVideo5,
      techArchitecture: architecture5,
      benefits: "Adoption: - By bringing together key capabilities for developers, IT ops, QA, and admins, our solution helps organizations deliver higher-quality software faster and more efficiently. It can help developers accelerate coding projects to boost productivity. DevOps engineers can create scripts to automate deployment pipelines for faster release cycles. Quality assurance specialists can use code analysis features to review code quality and adherence to best practices. Systems administrators can deploy monitoring and notification services to effectively manage infrastructure resources.",
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
    imageUrl: image5,
    title: "UK ADVISOR",
    description: "Code assistant streamlining code development through NL conversations for generation, review, and execution.",
    content: {
      description: "Code GReat (Generate, Review & Execute Code) The use of generative AI to assist with code development and deployment is garnering interest across organizations. Integrated development environment (IDE) tools like Amazon Code Whisperer and Microsoft GitHub Copilot that leverage large language models are being evaluated and utilized to accelerate development work. Our Code GReat solution utilizes a large language model-enabled graphical user interface to facilitate natural language interactions for code generation, review, and execution. Users can generate programming code, review it, and execute it in a conversational way. Code GReat aims to streamline the development process leveraging the power of generative AI.",
      solutionFlow: " The system can translate natural language prompts into source code across a variety of programming languages and infrastructure as code (IAC). It can perform code reviews and code hygiene checks to identify potential issues.  The system provides the option to leverage different large language models (LLMs) via integration with AWS services like Bedrock and SageMaker Jumpstart.  It can generate code for many scenarios, such as 'Generate a Terraform template to deploy an AWS EC2 instance with Load Balancer' or 'Bash command to find all files created by userA in Linux.  The system has the potential to be extended further to execute the generated code automatically, enabling full end-to-end infrastructure provisioning and configuration from natural language prompts.",
      demo: demoVideo5,
      techArchitecture: architecture5,
      benefits: "Adoption: - By bringing together key capabilities for developers, IT ops, QA, and admins, our solution helps organizations deliver higher-quality software faster and more efficiently. It can help developers accelerate coding projects to boost productivity. DevOps engineers can create scripts to automate deployment pipelines for faster release cycles. Quality assurance specialists can use code analysis features to review code quality and adherence to best practices. Systems administrators can deploy monitoring and notification services to effectively manage infrastructure resources.",
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
    imageUrl: image5,
    title: "UK ADVISOR",
    description: "Code assistant streamlining code development through NL conversations for generation, review, and execution.",
    content: {
      description: "Code GReat (Generate, Review & Execute Code) The use of generative AI to assist with code development and deployment is garnering interest across organizations. Integrated development environment (IDE) tools like Amazon Code Whisperer and Microsoft GitHub Copilot that leverage large language models are being evaluated and utilized to accelerate development work. Our Code GReat solution utilizes a large language model-enabled graphical user interface to facilitate natural language interactions for code generation, review, and execution. Users can generate programming code, review it, and execute it in a conversational way. Code GReat aims to streamline the development process leveraging the power of generative AI.",
      solutionFlow: " The system can translate natural language prompts into source code across a variety of programming languages and infrastructure as code (IAC). It can perform code reviews and code hygiene checks to identify potential issues.  The system provides the option to leverage different large language models (LLMs) via integration with AWS services like Bedrock and SageMaker Jumpstart.  It can generate code for many scenarios, such as 'Generate a Terraform template to deploy an AWS EC2 instance with Load Balancer' or 'Bash command to find all files created by userA in Linux.  The system has the potential to be extended further to execute the generated code automatically, enabling full end-to-end infrastructure provisioning and configuration from natural language prompts.",
      demo: demoVideo5,
      techArchitecture: architecture5,
      benefits: "Adoption: - By bringing together key capabilities for developers, IT ops, QA, and admins, our solution helps organizations deliver higher-quality software faster and more efficiently. It can help developers accelerate coding projects to boost productivity. DevOps engineers can create scripts to automate deployment pipelines for faster release cycles. Quality assurance specialists can use code analysis features to review code quality and adherence to best practices. Systems administrators can deploy monitoring and notification services to effectively manage infrastructure resources.",
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
    imageUrl: image5,
    title: "UK ADVISOR",
    description: "Code assistant streamlining code development through NL conversations for generation, review, and execution.",
    content: {
      description: "Code GReat (Generate, Review & Execute Code) The use of generative AI to assist with code development and deployment is garnering interest across organizations. Integrated development environment (IDE) tools like Amazon Code Whisperer and Microsoft GitHub Copilot that leverage large language models are being evaluated and utilized to accelerate development work. Our Code GReat solution utilizes a large language model-enabled graphical user interface to facilitate natural language interactions for code generation, review, and execution. Users can generate programming code, review it, and execute it in a conversational way. Code GReat aims to streamline the development process leveraging the power of generative AI.",
      solutionFlow: " The system can translate natural language prompts into source code across a variety of programming languages and infrastructure as code (IAC). It can perform code reviews and code hygiene checks to identify potential issues.  The system provides the option to leverage different large language models (LLMs) via integration with AWS services like Bedrock and SageMaker Jumpstart.  It can generate code for many scenarios, such as 'Generate a Terraform template to deploy an AWS EC2 instance with Load Balancer' or 'Bash command to find all files created by userA in Linux.  The system has the potential to be extended further to execute the generated code automatically, enabling full end-to-end infrastructure provisioning and configuration from natural language prompts.",
      demo: demoVideo5,
      techArchitecture: architecture5,
      benefits: "Adoption: - By bringing together key capabilities for developers, IT ops, QA, and admins, our solution helps organizations deliver higher-quality software faster and more efficiently. It can help developers accelerate coding projects to boost productivity. DevOps engineers can create scripts to automate deployment pipelines for faster release cycles. Quality assurance specialists can use code analysis features to review code quality and adherence to best practices. Systems administrators can deploy monitoring and notification services to effectively manage infrastructure resources.",
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
    imageUrl: image5,
    title: "UK ADVISOR",
    description: "Code assistant streamlining code development through NL conversations for generation, review, and execution.",
    content: {
      description: "Code GReat (Generate, Review & Execute Code) The use of generative AI to assist with code development and deployment is garnering interest across organizations. Integrated development environment (IDE) tools like Amazon Code Whisperer and Microsoft GitHub Copilot that leverage large language models are being evaluated and utilized to accelerate development work. Our Code GReat solution utilizes a large language model-enabled graphical user interface to facilitate natural language interactions for code generation, review, and execution. Users can generate programming code, review it, and execute it in a conversational way. Code GReat aims to streamline the development process leveraging the power of generative AI.",
      solutionFlow: " The system can translate natural language prompts into source code across a variety of programming languages and infrastructure as code (IAC). It can perform code reviews and code hygiene checks to identify potential issues.  The system provides the option to leverage different large language models (LLMs) via integration with AWS services like Bedrock and SageMaker Jumpstart.  It can generate code for many scenarios, such as 'Generate a Terraform template to deploy an AWS EC2 instance with Load Balancer' or 'Bash command to find all files created by userA in Linux.  The system has the potential to be extended further to execute the generated code automatically, enabling full end-to-end infrastructure provisioning and configuration from natural language prompts.",
      demo: demoVideo5,
      techArchitecture: architecture5,
      benefits: "Adoption: - By bringing together key capabilities for developers, IT ops, QA, and admins, our solution helps organizations deliver higher-quality software faster and more efficiently. It can help developers accelerate coding projects to boost productivity. DevOps engineers can create scripts to automate deployment pipelines for faster release cycles. Quality assurance specialists can use code analysis features to review code quality and adherence to best practices. Systems administrators can deploy monitoring and notification services to effectively manage infrastructure resources.",
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
];


import React, { useRef } from "react";
import { BrowserRouter as Router, Routes, Route, Link } from "react-router-dom";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faArrowRight, faArrowLeft, faChevronDown } from "@fortawesome/free-solid-svg-icons";
import Header from "./components/Header/Header";
import MyCarousel from "./components/Carousel/MyCarousel";
import Cards from "./components/Cards/Cards";
import styles from "./App.module.css";
import SideBarPage from "./components/Sidebar/SideBarPage";
import AllCardsPage from "./components/Cards/AllCardsPage"; // Import the new component
import { cardsData } from "./data"; // Import card data from a separate file

const App = () => {
  const [bigIndex, setBigIndex] = React.useState(null);
  const cardsContainerRef = useRef(null);

  const toggleSize = (index) => {
    setBigIndex(index === bigIndex ? null : index);
  };

  const handleClickLeft = () => {
    if (bigIndex === null || bigIndex === 0) {
      setBigIndex(cardsData.length - 1);
    } else {
      setBigIndex(bigIndex - 1);
    }
  };

  const handleClickRight = () => {
    if (bigIndex === null || bigIndex === cardsData.length - 1) {
      setBigIndex(0);
    } else {
      setBigIndex(bigIndex + 1);
    }
  };

  const handleScrollDown = () => {
    cardsContainerRef.current.scrollIntoView({ behavior: "smooth" });
  };

  return (
    <Router>
      <div className={styles.app}>
        <Header />
        <Routes>
          <Route
            path="/"
            element={
              <Home
                cardsData={cardsData}
                handleClickLeft={handleClickLeft}
                handleClickRight={handleClickRight}
                bigIndex={bigIndex}
                toggleSize={toggleSize}
                cardsContainerRef={cardsContainerRef}
              />
            }
          />
          <Route path="/dashboard" element={<SideBarPage />} />
          <Route path="/all-cards" element={<AllCardsPage cardsData={cardsData} />} /> {/* Pass cardsData to AllCardsPage */}
        </Routes>
        <div className={styles.scrollDownButton} onClick={handleScrollDown} title="Go Down">
          <FontAwesomeIcon icon={faChevronDown} />
        </div>
      </div>
    </Router>
  );
};

const Home = ({
  cardsData,
  handleClickLeft,
  handleClickRight,
  bigIndex,
  toggleSize,
  cardsContainerRef,
}) => {
  return (
    <>
      <MyCarousel />
      <div className={styles.cardsContainer} ref={cardsContainerRef}>
        <div className={styles.viewAllContainer}>
          <Link to="/all-cards" className={styles.viewAllButton}>
            View All Solutions
          </Link>
        </div>
        <span
          className={`${styles.arrow} ${styles.leftArrow}`}
          onClick={handleClickLeft}
        >
          <FontAwesomeIcon icon={faArrowLeft} title="Previous"/>
        </span>
        {cardsData.slice(0, 5).map((card, index) => (
          <Cards
            key={index}
            imageUrl={card.imageUrl}
            title={card.title}
            description={card.description}
            isBig={index === bigIndex}
            toggleSize={() => toggleSize(index)}
          />
        ))}
        <span
          className={`${styles.arrow} ${styles.rightArrow}`}
          onClick={handleClickRight}
        >
          <FontAwesomeIcon icon={faArrowRight} title="Next"/>
        </span>
      </div>
    </>
  );
};

export default App;

@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600;700&display=swap");

/* Hide the scrollbar */
::-webkit-scrollbar {
  width: 0; /* Remove scrollbar width */
  height: 0; /* Remove scrollbar height */
}

html,
body {
  overflow-y: scroll; /* Always show vertical scrollbar */
  font-family: "Poppins", sans-serif;
}

.app {
  width: 1100px;
  height: 600px;
  margin: 0 auto;
}

.cardsContainer {
  gap: 20px;
  border-radius: 12px;
  display: flex;
  justify-content: center;
}

.arrow {
  cursor: pointer;
  position: relative;
  top: 50%;
  transform: translateY(-50%);
  font-size: 18px;
  width: 17px;
    height: 22px;
    padding: 3px 8px 5px 6px;
  border-radius: 50px;
  border: 2px solid rgba(15, 95, 220, 1);
  color: rgba(15, 95, 220, 1); /* Adjust color as needed */
  transition: transform 0.5s ease, background 0.5s ease; /* Add transition */
}

.arrow:hover {
  background-color: rgba(15, 95, 220, 1); /* Change background color on hover */
  color: white; /* Change text color on hover */
}

.leftArrow {
  left: -25px;
  top: 90px;
}

.rightArrow {
  right: -25px;
  top: 90px;
}

@media screen and (max-width: 1100px) {
  .app {
    padding: 0 10px;
  }
}

@media screen and (max-width: 768px) {
  .app {
    max-width: 100%;
  }
}


.arrow img::after {
  content: attr(title);
  position: absolute;
  top: 100%;
  left: 50%;
  transform: translateX(-50%);
  background-color: #333;
  color: #fff;
  padding: 4px;
  border-radius: 5px;
  font-size: 8px;
  white-space: nowrap;
  opacity: 0;
  visibility: hidden;
  /*transition: opacity 0.1s, visibility 0.1s;*/
}

.logo img:hover::after {
  opacity: 1;
  visibility: visible;
}

.viewAllContainer {
  width: 100%;
  display: flex;
  justify-content: right;
  align-items: center;
  /*margin-bottom: 20px;*/
  margin-right: 123px;
}

.solutionHead{
    font-weight: 600;
    font-size: 14px;
    color: #808080;
      margin-left: 123px;

}


.viewAllButton {
   font-weight: 600;
    font-size: 14px;
    color: #808080;
  cursor: pointer;
  transition: background-color 0.3s ease, transform 0.3s ease;
  /*text-decoration: none;*/
}


.viewAllButton:hover {
  /*background-color: rgba(13, 85, 198, 1);*/
  transform: translateY(-2px);
  color:#000;
}

.cardsContainer {
  gap: 20px;
  border-radius: 12px;
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
}

.scrollDownButton {
  position: fixed;
  left: 20px; /* Adjust as per your layout */
  bottom: 20px; /* Adjust as per your layout */
  background: linear-gradient(90deg, #6f36cd 0%, #1f77f6 100%);
  color: white;
  border: none;
  cursor: pointer;
  border-radius: 4px;
  width: 30px;
  height: 40px;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 20px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.scrollDownButton:hover {
  background-color: rgba(13, 85, 198, 1); /* Darker shade on hover */
}



import React, { useState, useEffect } from "react";
import { Link } from "react-router-dom";
import styles from "./Cards.module.css";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faArrowRight } from "@fortawesome/free-solid-svg-icons";

const Card = ({ imageUrl, title, description, isBig, toggleSize }) => {
  const [isHovered, setIsHovered] = useState(false);

  useEffect(() => {
    window.scrollTo({ top: 0, behavior: "smooth" });
  }, []);

  return (
    <div className={styles.cardsContainer}>
      <div
        className={`${styles.card} ${isBig ? styles.big : ""}`}
        style={{ backgroundImage: `url(${imageUrl})` }}
        onClick={toggleSize}
      >
        {!isBig && <div className={styles.cardTitle}>{title}</div>}
        {isBig && (
          <div className={styles.cardContent}>
            <h2>{title}</h2>
            <p>{description}</p>
            <Link
              to={{
                pathname: "/dashboard",
                search: `?title=${encodeURIComponent(title)}`,
              }}
              className={styles.readMoreLink}
            >
              <span
                className={`${styles.arrow} ${styles.rightArrow} ${
                  isHovered ? styles.hovered : ""
                }`}
                onMouseEnter={() => setIsHovered(true)}
                onMouseLeave={() => setIsHovered(false)}
              >
                <span
                  style={{
                    fontSize: isHovered ? "0.8em" : "1em",
                  }}
                >
                  {isHovered && "Read More "}
                </span>
                <span
                  style={{
                    marginLeft: isHovered ? "5px" : "0",
                    fontSize: isHovered ? "1.3em" : "1em",
                  }}
                >
                  <FontAwesomeIcon icon={faArrowRight} />
                </span>
              </span>
            </Link>
          </div>
        )}
      </div>
    </div>
  );
};

export default Card;

.card {
  width: 160px;
  overflow: hidden;
  height: 160px;
  border-radius: 12px;
  background-size: cover;
  background-position: center;
  cursor: pointer;
  position: relative;
  margin-bottom: 90px;
  transition: transform 0.6s ease;
}

.card:hover{
  transform: translate(0, -10px);
}

.big {
  width: 190px;
  height: 190px;
  z-index: 0;
  transform: scale(1.1);
}

.cardTitle {
  position: absolute;
  font-family: "Poppins", sans-serif;
  font-size: 12px;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 75px; /* Set a fixed height */
  background: linear-gradient(180deg, rgba(0, 0, 0, 0) 0%, #000000 100%),
    linear-gradient(0deg, rgba(23, 23, 25, 0.3), rgba(23, 23, 25, 0.3));
  backdrop-filter: blur(10px); /* Adjust the blur radius as needed */
  border-radius: 12px;
  color: white;
  padding: 18px;
  box-sizing: border-box;
  transition: opacity 0.3s ease;
  text-align: center;
}

.cardContent {
  position: absolute;
  font-family: "Poppins", sans-serif;
  font-size: 8px;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 140px;
  background: linear-gradient(180deg, rgba(0, 0, 0, 0) 0%, #000000 100%),
    linear-gradient(0deg, rgba(23, 23, 25, 0.3), rgba(23, 23, 25, 0.3));
  border-radius: 12px;
    backdrop-filter: blur(10px); /* Adjust the blur radius as needed */

  color: white;
  padding: 18px;
  box-sizing: border-box;
  transform: translateY(100%);
  transition: transform 0.3s ease, background 0.3s ease; /* Adjusted transition timing */
}

.cardContent.slide-up {
  transform: translateY(0);
}

.cardContent::before {
  content: "";
  position: absolute;
  top: 100%;
  left: 0;
  width: 100%;
  height: 100%;
  background: linear-gradient(to top, #6f36cd, #1f77f6);
  transition: top 0.3s ease;
  border-radius: 12px;
  z-index: -1;
}
.arrow {
 cursor: pointer;
    position: absolute;
    display: flex;
    bottom: 10px;
    right: 10px;
    font-size: 14px;
    width: 7px;
    height: 23px;
    padding: 2px 12px 0px 6px;
    border-radius: 50px;
    border: 2px solid rgba(255, 255, 255, 1);
    color: rgba(255, 255, 255, 1);
    overflow: hidden;
    transition: width 0.3s ease, background 0.5s ease, color 0.5s ease;
}

.arrow:hover {
  width: 80px; /* Increase width on hover */
  align-items: center;
  font-size: 10px;
  background-color: rgba(
    255,
    255,
    255,
    1
  ); /* Change background color on hover */
  color: rgba(15, 95, 220, 1); /* Change text color on hover */
}

.arrow.hovered {
  width: 65px;
}
.card:hover .cardContent::before {
  top: 0;
}
.readMore {
  left: 10px;
}
.card:hover .cardContent::before {
  background: linear-gradient(0deg, #6f36cd 0%, #1f77f6 100%);
}

.big .cardContent {
  transform: translateY(0);
}

import React from "react";
import styles from "./AllCardsPage.module.css"; // Create a new CSS module for styling
import Cards from "./Cards";
import { cardsData } from "../../data"; // Import cardsData from the data file
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faArrowLeft } from "@fortawesome/free-solid-svg-icons";

const AllCardsPage = ({ cardsData }) => {
  
  const handleBackButtonClick = () => {
    window.history.back(); // Go back to the previous page
  };
  return (
    <div className={styles.allCardsPage}>
<button onClick={handleBackButtonClick} className={styles.backButton}>
          <FontAwesomeIcon icon={faArrowLeft} />
        </button>
      <div className={styles.allCardsContainer}>
        {cardsData.map((card, index) => (
          <Cards
            key={index}
            imageUrl={card.imageUrl}
            title={card.title}
            description={card.description}
            isBig={false} // Ensure all cards are in the default small size
          />
        ))}
      </div>
    </div>
  );
};

export default AllCardsPage;

.allCardsPage {
  padding: 20px;
  margin-top: 40px;
  
}

.backButton {
  background-color: rgba(230, 235, 245, 1);
    padding: 8px;
    /*margin-left: 20px;*/
    margin-bottom: 10px;
    border-radius: 4px;
    width: 40px;
    font-size: 14px;
    border: none;
    cursor: pointer;
    margin-right: 30px;
    position: fixed;
    left:40px;
}

backIcon{
  font-size: 12px;
}

.backButton:hover {
  color: rgba(95, 30, 193, 1); /* Change button color on hover */
}

.allCardsContainer {
  display: grid;
  grid-template-columns: repeat(5, 1fr); /* Five cards per row */
  /*gap: 20px;*/
  padding-top: 50px;
}

@media (max-width: 1200px) {
  .allCardsContainer {
    grid-template-columns: repeat(4, 1fr); /* Four cards per row on smaller screens */
  }
}

@media (max-width: 900px) {
  .allCardsContainer {
    grid-template-columns: repeat(3, 1fr); /* Three cards per row on even smaller screens */
  }
}

@media (max-width: 600px) {
  .allCardsContainer {
    grid-template-columns: repeat(2, 1fr); /* Two cards per row on mobile devices */
  }
}

@media (max-width: 400px) {
  .allCardsContainer {
    grid-template-columns: 1fr; /* One card per row on very small screens */
  }
}
