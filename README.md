function mapAssets(card) {
  return {
    ...card,
    imageUrl: card.imageUrl ? images[card.imageUrl.split('.').pop()] : null,
    content: {
      ...card.content,
      solutionFlow: Array.isArray(card.content.solutionFlow) 
        ? card.content.solutionFlow.map(flow => solutionFlows[flow.split('.').pop()]) 
        : [],
      demo: card.content.demo ? videos[card.content.demo.split('.').pop()] : null,
      techArchitecture: card.content.techArchitecture ? architectures[card.content.techArchitecture.split('.').pop()] : null,
      descriptionFlow: card.content.description ? descriptions[card.content.description.split('.').pop()] : null,
      benefitsFlow: card.content.benefits ? solutionsBenefits[card.content.benefits.split('.').pop()] : null,
      adoptionFlow: card.content.adoption ? adoption[card.content.adoption.split('.').pop()] : null,
    },
  };
}


{
  "imageUrl": "images.CitizenAdvisor",
  "title": "Citizen Advisor",
  "description": "An experience transformation from disconnected silos information to an intuitive, personalized revelations",
  "industry": "GOVT",
  "businessFunction": "Customer Experience",
  "content": {
    "description": "descriptions.DescriptionDemo",
    "solutionFlow": ["solutionFlows.CitizenAdvisorFlow1", "solutionFlows.CitizenAdvisorFlow2", "solutionFlows.CitizenAdvisorFlow3", "solutionFlows.CitizenAdvisorFlow4"],
    "demo": "videos.demoVideo5",
    "techArchitecture": "architectures.CitizenAdvisorArchitecture",
    "benefits": "solutionsBenefits.citizenBenefits",
    "adoption": "adoption.citizenAdoption"
  }
}



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
        <Carousel showArrows={false} showIndicators={false}>
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
        <img
          src={content.benefitsFlow}
          alt="Benefits Flow"
          className={maximizedImage === content.benefitsFlow ? styles.maximized : ""}
          onClick={() => toggleMaximize(content.benefitsFlow)}
        />
      </div>
    ),
    adoption: (
      <div className={styles.adoption}>
        <h2>Solution Adoption</h2>
        <img
          src={content.adoptionFlow}
          alt="Adoption Flow"
          className={maximizedImage === content.adoptionFlow ? styles.maximized : ""}
          onClick={() => toggleMaximize(content.adoptionFlow)}
        />
      </div>
    ),
  };

  return (
    <div className={styles.mainContent}>
      {contentMap[activeTab] || <div>Content not available</div>}
      {maximizedImage && (
        <div className={styles.overlay} onClick={() => setMaximizedImage(null)}>
          <FontAwesomeIcon icon={faTimes} className={styles.closeIcon} onClick={() => setMaximizedImage(null)} />
          <img src={maximizedImage} alt="Maximized view" className={styles.maximized} />
        </div>
      )}
    </div>
  );
};

export default MainContent;
