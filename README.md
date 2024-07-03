
// src/components/MainContent.js
import React from "react";
import styles from "./MainContent.module.css";
import SolutionImg from "./Icons/solutionFlowGraph.png";
import TechnicalArchitecture from "./Icons/technicalArchitecture.png";
import videoDemoUrl from "./Icons/Email-EAR.mp4";
import Video from "./Video";

const MainContent = ({ activeTab }) => {
  const renderContent = () => {
    switch (activeTab) {
      case "description":
        return <div className={styles.content}>Description content goes here</div>;
      case "solutionFlow":
        return (
          <div className={styles.content}>
            <img src={SolutionImg} alt="Solution Flow" className={styles.image} />
          </div>
        );
      case "demo":
        return (
          <div className={styles.content}>
            <Video src={videoDemoUrl} />
          </div>
        );
      case "techArchitecture":
        return (
          <div className={styles.content}>
            <img src={TechnicalArchitecture} alt="Technical Architecture" className={styles.image} />
          </div>
        );
      case "benefits":
        return <div className={styles.content}>Benefits and Use Cases content goes here</div>;
      default:
        return null;
    }
  };

  return <div className={styles.mainContent}>{renderContent()}</div>;
};

export default MainContent;




// src/components/Video.js
import React from "react";
import PropTypes from "prop-types";
import styles from "./Video.module.css";

const Video = ({ src }) => (
  <video className={styles.video} controls>
    <source src={src} type="video/mp4" />
    Your browser does not support the video tag.
  </video>
);

Video.propTypes = {
  src: PropTypes.string.isRequired,
};

export default Video;





/* src/components/MainContent.module.css */
.mainContent {
  padding: 20px;
}

.content {
  margin-bottom: 20px;
}

.image {
  width: 100%;
  height: auto;
}

.video {
  width: 100%;
  height: auto;
}
