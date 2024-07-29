import React, { useState } from "react";
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

  const descriptionImages = content.descriptionImages ? content.descriptionImages.map((imgSrc, index) => (
    <img
      key={index}
      src={imgSrc}
      alt={`Description ${index}`}
      className={maximizedImage === imgSrc ? styles.maximized : ""}
      onClick={() => toggleMaximize(imgSrc)}
    />
  )) : [];

  const solutionFlowRows = content.solutionFlowText?.map((row, index) => (
    <tr key={index}>
      <td>{row.step}</td>
      <td>{row.description}</td>
    </tr>
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
        {descriptionImages}
      </div>
    ),
    solutionFlow: (
      <div className={styles.solution}>
        <h2>Solution Flow</h2>
        <img
          src={content.solutionFlow}
          alt="Solution Flow"
          className={maximizedImage === content.solutionFlow ? styles.maximized : ""}
          onClick={() => toggleMaximize(content.solutionFlow)}
        />
        {solutionFlowRows && (
          <table className={styles.adoptionTable}>
            <thead>
              <tr>
                <th>Steps</th>
                <th>Description</th>
              </tr>
            </thead>
            <tbody>{solutionFlowRows}</tbody>
          </table>
        )}
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
          {content.benefits.split(". ").map((point, index) => (
            <li key={index} dangerouslySetInnerHTML={{ __html: point.trim() }}></li>
          ))}
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
          <FontAwesomeIcon icon={faTimes} className={styles.closeIcon} />
          <img src={maximizedImage} alt="Maximized view" className={styles.maximized} />
        </div>
      )}
    </div>
  );
};

export default MainContent;





{
  "descriptionImages": [
    "path/to/descriptionImage1.jpg",
    "path/to/descriptionImage2.jpg"
  ],
  "solutionFlow": "path/to/solutionFlowImage.png",
  "demo": "path/to/demoVideo.mp4",
  "techArchitecture": "path/to/techArchitectureImage.png",
  "benefitsImg": "path/to/benefitsImage.png",
  "adoption": [
    {
      "industry": "Industry 1",
      "adoption": "Adoption 1"
    },
    {
      "industry": "Industry 2",
      "adoption": "Adoption 2"
    }
  ],
  "solutionFlowText": [
    {
      "step": "Step 1",
      "description": "Description for step 1"
    },
    {
      "step": "Step 2",
      "description": "Description for step 2"
    }
  ]
}





// AssetImports.js
export const images = {
  // Example: Use appropriate file paths or URLs
  IntelligentAss: require('./components/Cards/CardsImages/card3.jpg'),
  EmailEAR: require('./components/Cards/CardsImages/card1.jpg'),
  // Add other image paths
};

export const videos = {
  EmailEARDemo: 'https://example.com/demovideos/Email-EAR_Demo_new.mp4',
  // Add other video URLs
};

export const solutionFlows = {
  EmailEarFlow: require('./components/Sidebar/Icons/EmailEarFlowGraph.png'),
  // Add other solution flow paths
};

export const architectures = {
  IntelligentAssistArchitecture: require('./components/Sidebar/Icons/IntelligentAssistarchitecture.png'),
  // Add other architecture paths
};
