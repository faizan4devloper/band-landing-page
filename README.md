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
];

import React from "react";
import { BrowserRouter as Router, Routes, Route } from "react-router-dom";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faArrowRight, faArrowLeft } from "@fortawesome/free-solid-svg-icons";
import Header from "./components/Header/Header";
import MyCarousel from "./components/Carousel/MyCarousel";
import Cards from "./components/Cards/Cards";
// import Footer from "./components/Footer/Footer";
import styles from "./App.module.css";
import SideBarPage from "./components/Sidebar/SideBarPage";
import { cardsData } from "./data"; // Import card data from a separate file

const App = () => {
  const [bigIndex, setBigIndex] = React.useState(null);

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
  
  // Get the app container
const appContainer = document.querySelector('.app');

// Add event listener to the card elements
const cards = document.querySelectorAll('.card');
cards.forEach(card => {
  card.addEventListener('click', () => {
    // Scroll the page up to the app container
    appContainer.scrollIntoView({ behavior: 'smooth', block: 'start' });
  });
});

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
              />
            }
          />
          <Route path="/dashboard" element={<SideBarPage />} />
        </Routes>
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
}) => {
  return (
    <>
      <MyCarousel />
      <div className={styles.cardsContainer}>
        <span
          className={`${styles.arrow} ${styles.leftArrow}`}
          onClick={handleClickLeft}
        >
<FontAwesomeIcon icon={faArrowLeft} title="Previous"/>           </span>
        {cardsData.map((card, index) => (
          <Cards
            key={index}
            imageUrl={card.imageUrl}
            title={card.title}
            description={card.description}
            isBig={index === bigIndex}
            toggleSize={() => toggleSize(index)}
            onClick={Cards}
          />
        ))}
        <span
          className={`${styles.arrow} ${styles.rightArrow}`}
          onClick={handleClickRight}
        >
<FontAwesomeIcon icon={faArrowRight} title="Next"/>           </span>
      </div>
    </>
  );
};

export default App;

// components/Cards/Cards.js
import React, { useState } from "react";
import { Link } from "react-router-dom"; // Import Link component
import styles from "./Cards.module.css"; // Import CSS module for card styles
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faArrowRight } from "@fortawesome/free-solid-svg-icons";

const Card = ({ imageUrl, title, description, isBig, toggleSize }) => {
  const [isHovered, setIsHovered] = useState(false);

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
            {/* Add Link component to navigate to dashboard with title query parameter */}
            <Link
              to={{
                pathname: "/dashboard",
                search: `?title=${encodeURIComponent(title)}`,
              }}
              className={styles.readMoreLink}
            >
              {/* Content of the "Read More" link */}
              <span
                className={`${styles.arrow} ${styles.rightArrow} ${
                  isHovered ? styles.hovered : ""
                }`}
                onMouseEnter={() => setIsHovered(true)}
                onMouseLeave={() => setIsHovered(false)}
              >
                <span
                  style={{
                    fontSize: isHovered ? "0.8em" : "1em", // Decrease font size on hover
                  }}
                >
                  {isHovered && "Read More "}
                </span>
                <span
                  style={{
                    marginLeft: isHovered ? "5px" : "0", // Add space on hover
                    fontSize: isHovered ? "1.3em" : "1em", // Increase size on hover
                  }}
                >
<FontAwesomeIcon icon={faArrowRight} />                </span>
              </span>
            </Link>
          </div>
        )}
      </div>
    </div>
  );
};

export default Card;
