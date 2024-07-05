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
import solutionFlow2 from "./components/Sidebar/Icons/solutionFlowGraph2.png";
import solutionFlow3 from "./components/Sidebar/Icons/solutionFlowGraph3.png";
import solutionFlow4 from "./components/Sidebar/Icons/solutionFlowGraph4.png";
import solutionFlow5 from "./components/Sidebar/Icons/solutionFlowGraph5.png";

import architecture1 from "./components/Sidebar/Icons/technicalArchitecture1.png";
import architecture2 from "./components/Sidebar/Icons/technicalArchitecture2.png";
import architecture3 from "./components/Sidebar/Icons/technicalArchitecture3.png";
import architecture4 from "./components/Sidebar/Icons/technicalArchitecture4.png";
import architecture5 from "./components/Sidebar/Icons/technicalArchitecture5.png";

export const cardsData = [
  {
    imageUrl: image1,
    title: "Email EAR",
    description: "The Email EAR solution automates email customer support, enhancing customer experience.",
    content: {
      description: "Email EAR (Extract, Act and Respond) In today’s era, there are unprecedented ways of engaging the customer and this includes both traditional and digital channels. Unified experience across channels is key for customer delight. Despite the rise of alternative options, Email remains a preferred communication channel for customer support. Email provides unique benefits like maintaining a history of interactions, allowing customers to comprehensively explain issues, and enabling file attachments for additional context. However, the prioritization and queuing of emails often leads to delayed responses that create poor customer experiences. Our Gen AI-powered Email EAR (Extract, Act, and Respond) solution aims to transform the customer support process by automating the reading, analysis, and thoughtful responding to incoming emails. Specifically, our system can extract the core query, complaint, or issue within an email. It then understands what actions are required to resolve the customer's needs. Finally, it generates a user-friendly, detailed response explaining steps taken to address their questions or concerns.",
      solutionFlow: solutionFlow1,
      demo: demoVideo1,
      techArchitecture: architecture1,
      benefits: "This solution assists organizations in enhancing the customer experience for email responses. Automating responses to customer queries or integrating with applications to retrieve data or generate tickets facilitates completing the overall process without delays. Timely and accurate responses help in increased self-service, reduced customer service inquiries, and higher customer satisfaction. Human review of generated responses allows validation of customer feedback and enhancement of the knowledge base via feedback-driven updates. Overall agent productivity stands to improve, as the solution can be seen as an agent assist that enables reviewing automatically generated responses and interacting with customers accordingly. The adaptability of the solution allows it to be implemented across various verticals to address common customer pain points.",
      industryAdoption: [
        {
          industry: "Financial",
          adoption: "Can help in answering finance product information, Loan eligibility, Product Recommendation as per the email context"
        },
        {
          industry: "Education",
          adoption: "Can help students to get response to their queries related to Admission process, Scholarship schemes, Enrollment process etc."
        },
        {
          industry: "Healthcare/Insurance",
          adoption: "Can help customers to understand their health insurance eligibility, claim processing, claim status etc."
        },
        {
          industry: "Retail",
          adoption: "Queries related to product exchange / return can be better handled"
        }
      ]
    }
  },
  {
    imageUrl: image2,
    title: "Signature Extraction & Verification",
    description: "Signature extraction, verification, unique identification, document authorization, security, efficiency, AI/ML.",
    content: {
      description: "Signature Extraction & Verification Signatures are unique to each person and can be used to verify identity and authorize documents. Extracting signatures from documents and verifying their authenticity is crucial for security, preventing forgery and ensuring validity of legal agreements. AI/ML algorithms can analyze shape, stroke style and pressure to accurately match signatures. Automating extraction and verification improves efficiency and allows large volumes of documents to be quickly processed. Below is the expected outcome from the solution:- The proposed system will have the ability to search through scanned or faxed multi-page documents to identify and extract any handwritten signatures present. Once a signature is extracted, it can be compared against reference signatures stored in an Imaging Signature Verification database to determine if the extracted signature matches or mismatches a known signature.",
      solutionFlow: solutionFlow2,
      demo: demoVideo2,
      techArchitecture: architecture2,
      benefits: "The purpose of this solution is to enhance the customer experience, making it adaptable across various industries. Specifically, it helps customers better understand the products and services an organization offers.",
      industryAdoption: [
        {
          industry: "Financial",
          adoption: "Can help in financial document verification, ensuring security and authenticity."
        },
        {
          industry: "Legal",
          adoption: "Ensuring document authenticity and legal compliance through signature verification."
        },
        {
          industry: "Healthcare",
          adoption: "Verification of medical documents and patient records."
        },
        {
          industry: "Government",
          adoption: "Verification of identity documents and official paperwork."
        }
      ]
    }
  },
  {
    imageUrl: image3,
    title: "Intelligent Assist",
    description: "GenAI Intelligent Assist(Q&A) supports global workforce with multilingual knowledge access.",
    content: {
      description: "GenAI Intelligent Assist(Q&A) Every organization possesses a vast knowledge base embedded within its extensive document repositories. This knowledge is critical for supporting both customers and internal employees in comprehending organizational processes, products, policies, and more. For large, multinational corporations with a diverse, multi-lingual workforce, Gen AI-powered intelligent assistants can serve as highly effective support agents by accurately retrieving precise information from these knowledge sources at scale. Our Gen AI Intelligent Assist leverages generative intelligence to efficiently search across an organization's documents and provide accurate, relevant information to questions or requests. With multi-lingual capabilities, it can serve global workforces by delivering the right knowledge to the right people when they need it, regardless of language or location.",
      solutionFlow: solutionFlow3,
      demo: demoVideo3,
      techArchitecture: architecture3,
      benefits: "The purpose of this solution is to enhance the customer experience, making it adaptable across various industries. Specifically, it helps customers better understand the products and services an organization offers.",
      industryAdoption: [
        {
          industry: "Technology",
          adoption: "Improving internal knowledge base access and efficiency."
        },
        {
          industry: "Education",
          adoption: "Supporting student queries and educational material access."
        },
        {
          industry: "Healthcare",
          adoption: "Providing medical information and patient support."
        },
        {
          industry: "Retail",
          adoption: "Customer support and product information access."
        }
      ]
    }
  },
  {
    imageUrl: image4,
    title: "Case Intelligence",
    description: "GenAI automates pharmaceutical case intake, streamlining manual processes and enhancing data capture.",
    content: {
      description: "GenAI Case Intelligence Pharmaceutical companies around the world typically offer patient support services where patients can report medication side effects, usage, and symptoms. Patients provide this information through phone calls, emails, written forms, and other manual channels. Managing these manual intake processes can be time-consuming and may lead to missed key details during information exchange. Our Gen AI-powered Case Intelligence solution automates case intake from various channels like email, call transcripts, and handwritten forms. This streamlines the process and ensures no critical information is overlooked, regardless of how patients submit their reports.",
      solutionFlow: solutionFlow4,
      demo: demoVideo4,
      techArchitecture: architecture4,
      benefits: "Industry Adoption:- Although originally designed for the life sciences and healthcare industries, the approach and framework presented in this solution can be readily customized and adopted across various industries.",
      industryAdoption: [
        {
          industry: "Healthcare",
          adoption: "Automating case intake and patient support services."
        },
        {
          industry: "Pharmaceuticals",
          adoption: "Streamlining medication reporting and data capture."
        },
        {
          industry: "Insurance",
          adoption: "Automating claims processing and customer support."
        },
        {
          industry: "Legal",
          adoption: "Document management and case processing automation."
        }
      ]
    }
  },
  {
    imageUrl: image5,
    title: "Code GReat",
    description: "The Code GReat solution utilizes generative AI to assist with code development and deployment.",
    content: {
      description: "Code GReat (Generate, Review & Execute Code) The use of generative AI to assist with code development and deployment is garnering interest across organizations. Integrated development environment (IDE) tools like Amazon Code Whisperer and Microsoft GitHub Copilot that leverage large language models are being evaluated and utilized to accelerate development work. Our Code GReat solution utilizes a large language model-enabled graphical user interface to facilitate natural








import React from "react";
import styles from "./MainContent.module.css";
import Video from "./Video";

const MainContent = ({ activeTab, content }) => {
  // Ensure content object and description are defined
  if (!content || !content.description) {
    return <div className={styles.mainContent}>Description not available</div>;
  }

  // Splitting the description by periods followed by spaces
  const descriptionPoints = content.description.split(". ").map((point, index) => (
    <li key={index}>{point.trim()}</li>
  ));
  const benefitsPoints = content.benefits.split(". ").map((point, index) => (
    <li key={index}>{point.trim()}</li>
  ));

  // Function to render industry adoption table
  const renderIndustryAdoption = () => {
    if (!content.industryAdoption) return null;

    return (
      <div className={styles.industryAdoption}>
        <h2>Industry Adoption</h2>
        <table>
          <thead>
            <tr>
              <th>Industry</th>
              <th>Solution Adoption</th>
            </tr>
          </thead>
          <tbody>
            {content.industryAdoption.map((item, index) => (
              <tr key={index}>
                <td>{item.industry}</td>
                <td>{item.adoption}</td>
              </tr>
            ))}
          </tbody>
        </table>
      </div>
    );
  };

  const contentMap = {
    description: (
      <div className={styles.description}>
        <h2>Description</h2>
        <ul>
          {descriptionPoints}
        </ul>
      </div>
    ),
    solutionFlow: (
      <div>
        <h2>Solution Flow</h2>
        <img src={content.solutionFlow} alt="Solution Flow" />
      </div>
    ),
    demo: (
      <div>
        <h2>Demo</h2>
        <Video src={content.demo} />
      </div>
    ),
    techArchitecture: (
      <div>
        <h2>Technical Architecture</h2>
        <img src={content.techArchitecture} alt="Technical Architecture" />
      </div>
    ),
    benefits: (
      <div className={styles.benefits}>
        <h2>Benefits</h2>
        <ul>
          {benefitsPoints}
        </ul>
      </div>
    ),
    industryAdoption: renderIndustryAdoption()
  };

  return (
    <div className={styles.mainContent}>
      {contentMap[activeTab] || <div>Content not available</div>}
    </div>
  );
};

export default MainContent;
