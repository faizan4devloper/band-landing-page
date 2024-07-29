import React, { useState } from "react";
import { Carousel } from "react-responsive-carousel";
import "react-responsive-carousel/lib/styles/carousel.min.css"; // Import carousel styles
import styles from "./MainContent.module.css";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faTimes } from "@fortawesome/free-solid-svg-icons";
import Video from "./Video";

const MainContent = ({ activeTab, content }) => {
  const [maximizedImage, setMaximizedImage] = useState(null);

  const toggleMaximize = (imageSrc) => {
    setMaximizedImage(maximizedImage === imageSrc ? null : imageSrc);
  };

  if (!content || !content.description) {
    return <div className={styles.mainContent}>Description not available</div>;
  }

  const keywords = [
    "extract", "Act", "Respond", "query", "complaint", "issue", "generates", "user-friendly", "questions", "concerns", "detailed response", "prioritization", "queuing", "delayed responses", "Gen AI-powered", "automating", "reading", "analysis", "thoughtful responding", "customer experience", "automates", "Gen AI-powered", "solution", "organization", "intelligent", "assist", "data capture", "manual processes", "Email EAR", "(Extract, Act and Respond)", "Unified experience"
  ];

  const highlightKeywords = (text) => {
    const regex = new RegExp(`\\b(${keywords.join("|")})\\b`, "gi");
    return text.replace(regex, (matched) => `<span class="${styles.highlight}">${matched}</span>`);
  };

  const descriptionPoints = content.description.split(". ").map((point, index) => (
    <li key={index} dangerouslySetInnerHTML={{ __html: highlightKeywords(point.trim()) }}></li>
  ));

  const benefitsPoints = content.benefits.split(". ").map((point, index) => (
    <li key={index} dangerouslySetInnerHTML={{ __html: highlightKeywords(point.trim()) }}></li>
  ));

  const adoptionRows = content.adoption.map((row, index) => (
    <tr key={index}>
      <td>{row.industry}</td>
      <td>{row.adoption}</td>
    </tr>
  ));

  const contentMap = {
    description: (
      <div className={styles.description}>
        <h2>Description</h2>
         <img
          src={content.descriptionFlow}
          alt="description Flow"
          className={maximizedImage === content.descriptionFlow ? styles.maximized : ""}
          onClick={() => toggleMaximize(content.descriptionFlow)}
        />
      </div>
    ),
    solutionFlow: (
      <div className={styles.solution}>
        <h2>Solution Flow</h2>
        <Carousel>
          {content.solutionFlow.map((image, index) => (
            <div key={index}>
              <img
                src={image}
                alt={`Solution Flow ${index + 1}`}
                className={maximizedImage === image ? styles.maximized : ""}
                onClick={() => toggleMaximize(image)}
              />
            </div>
          ))}
        </Carousel>
      </div>
    ),
    demo: (
      <div className={styles.demo}>
        <h2>Demo</h2>
        <Video src={content.demo} />
      </div>
    ),
    techArchitecture: (
      <div className={styles.architecture}>
        <h2>Technical Architecture</h2>
        <img
          src={content.techArchitecture}
          alt="Technical Architecture"
          className={maximizedImage === content.techArchitecture ? styles.maximized : ""}
          onClick={() => toggleMaximize(content.techArchitecture)}
        />
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
    adoption: (
      <div className={styles.adoption}>
        <h2>Solution Adoption</h2>
        <table className={styles.adoptionTable}>
          <thead>
            <tr>
              <th>Industry</th>
              <th>Solution Adoption</th>
            </tr>
          </thead>
          <tbody>{adoptionRows}</tbody>
        </table>
      </div>
    ),
  };

  return (
    <div className={styles.mainContent}>
      {contentMap[activeTab] || <div>Content not available</div>}
      {maximizedImage && (
        <div className={styles.overlay} onClick={() => setMaximizedImage(null)}>
          <FontAwesomeIcon icon={faTimes} className={styles.closeIcon} onClick={()=> setMaximizedImage(null)}/>
          <img src={maximizedImage} alt="Maximized view" className={styles.maximized} />
        </div>
      )}
    </div>
  );
};

export default MainContent;




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
