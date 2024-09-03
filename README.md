


import React, { useState, useEffect } from "react";
import { Link } from "react-router-dom";
import styles from "./Cards.module.css";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faArrowRight } from "@fortawesome/free-solid-svg-icons";

const Card = ({ imageUrl, title, description, isBig, toggleSize, tags }) => {
  const [isHovered, setIsHovered] = useState(false);

  useEffect(() => {
    window.scrollTo({ top: 0, behavior: "smooth" });
  }, []);

  // Merge tags into a single string separated by "/"
  const mergedTags = tags && tags.length > 1 ? tags.join(" / ") : tags?.[0];

  // List of titles where "Read More" should not be displayed
  const noReadMoreTitles = [
    "AAIG-API Analyzer & Insight Generator",
    "Responsible Gen AI with Llama-13 B",
    "Graph data Interpretation using Gen AI",
    "Predictive Asset Maintenance​(PAM)",
    "API based Test Case Generation",
    "SOP Assistance",
    "AMS Support Automation"
  ];

  return (
    <div className={styles.cardsContainer}>
      <div
        className={`${styles.card} ${isBig ? styles.big : ""}`}
        style={{ backgroundImage: `url(${imageUrl})` }}
        onClick={toggleSize}
      >
        {mergedTags && (
          <div className={styles.tagsContainer}>
            <span className={styles.tag}>{mergedTags}</span>
          </div>
        )}
        {!isBig && <div className={styles.cardTitle}>{title}</div>}
        {isBig && (
          <div className={styles.cardContent}>
            <h3>{title}</h3>
            <p>{description}</p>
            {/* Conditionally render the "Read More" link or "No Data Available" message */}
            {noReadMoreTitles.includes(title) ? (
              <p className={styles.noDataMessage}>No Data Available</p>
            ) : (
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
            )}
          </div>
        )}
      </div>
    </div>
  );
};

export default Card;



.noDataMessage {
  color: #ff4d4d; /* Red color for emphasis */
  font-size: 1em;
  font-weight: bold;
  margin-top: 10px;
  text-align: center;
  padding: 10px;
  border-radius: 5px;
  background-color: #ffe6e6; /* Light red background */
}
















import React, { useState, useEffect } from "react";
import { Link } from "react-router-dom";
import styles from "./Cards.module.css";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faArrowRight } from "@fortawesome/free-solid-svg-icons";

const Card = ({ imageUrl, title, description, isBig, toggleSize, tags }) => {
  const [isHovered, setIsHovered] = useState(false);

  useEffect(() => {
    window.scrollTo({ top: 0, behavior: "smooth" });
  }, []);

  // Merge tags into a single string separated by "/"
  const mergedTags = tags && tags.length > 1 ? tags.join(" / ") : tags?.[0];

  // List of titles where "Read More" should not be displayed
  const noReadMoreTitles = ["AAIG-API Analyzer & Insight Generator", "Responsible Gen AI with Llama-13 B", "Graph data Interpretation using Gen AI", "Predictive Asset Maintenance​(PAM) ", "API based Test Case Generation", "SOP Assistance", "AMS Support Automation"];

  return (
    <div className={styles.cardsContainer}>
      <div
        className={`${styles.card} ${isBig ? styles.big : ""}`}
        style={{ backgroundImage: `url(${imageUrl})` }}
        onClick={toggleSize}
      >
        {mergedTags && (
          <div className={styles.tagsContainer}>
            <span className={styles.tag}>{mergedTags}</span>
          </div>
        )}
        {!isBig && <div className={styles.cardTitle}>{title}</div>}
        {isBig && (
          <div className={styles.cardContent}>
            <h3>{title}</h3>
            <p>{description}</p>
            {/* Conditionally render the "Read More" link */}
            {!noReadMoreTitles.includes(title) && (
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
            )}
          </div>
        )}
      </div>
    </div>
  );
};

export default Card;
















const Card = ({ imageUrl, title, description, isBig, toggleSize, tags }) => {
  const [isHovered, setIsHovered] = useState(false);

  useEffect(() => {
    window.scrollTo({ top: 0, behavior: "smooth" });
  }, []);

  // Merge tags into a single string separated by "/"
  const mergedTags = tags && tags.length > 1 ? tags.join(" / ") : tags?.[0];

  // List of titles where "Read More" should not be displayed
  const noReadMoreTitles = ["AaigApi", "ResponsibleGen", "GraphData", "PredictiveAsset", "ApiCase"];

  return (
    <div className={styles.cardsContainer}>
      <div
        className={`${styles.card} ${isBig ? styles.big : ""}`}
        style={{ backgroundImage: `url(${imageUrl})` }}
        onClick={toggleSize}
      >
        {mergedTags && (
          <div className={styles.tagsContainer}>
            <span className={styles.tag}>{mergedTags}</span>
          </div>
        )}
        {!isBig && <div className={styles.cardTitle}>{title}</div>}
        {isBig && (
          <div className={styles.cardContent}>
            <h3>{title}</h3>
            <p>{description}</p>
            {/* Conditionally render the "Read More" link */}
            {!noReadMoreTitles.includes(title) && (
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
            )}
          </div>
        )}
      </div>
    </div>
  );
};















import React, { useState, useEffect } from "react";
import { Link } from "react-router-dom";
import styles from "./Cards.module.css";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faArrowRight } from "@fortawesome/free-solid-svg-icons";

const Card = ({ imageUrl, title, description, isBig, toggleSize, tags, solutionFlow, techArchitecture }) => {
  const [isHovered, setIsHovered] = useState(false);

  useEffect(() => {
    window.scrollTo({ top: 0, behavior: "smooth" });
  }, []);

  // Merge tags into a single string separated by "/"
  const mergedTags = tags && tags.length > 1 ? tags.join(" / ") : tags?.[0];

  // Check if the data for "Read More" is available
  const hasData = description || solutionFlow || techArchitecture;

  return (
    <div className={styles.cardsContainer}>
      <div
        className={`${styles.card} ${isBig ? styles.big : ""}`}
        style={{ backgroundImage: `url(${imageUrl})` }}
        onClick={toggleSize}
      >
        {mergedTags && (
          <div className={styles.tagsContainer}>
            <span className={styles.tag}>{mergedTags}</span>
          </div>
        )}
        {!isBig && <div className={styles.cardTitle}>{title}</div>}
        {isBig && (
          <div className={styles.cardContent}>
            <h3>{title}</h3>
            <p>{description}</p>
            {hasData ? (
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
            ) : (
              <div className={styles.noData}>No Data Available</div>
            )}
          </div>
        )}
      </div>
    </div>
  );
};

export default Card;












import React, { useState, useEffect } from "react";
import { Link } from "react-router-dom";
import styles from "./Cards.module.css";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faArrowRight } from "@fortawesome/free-solid-svg-icons";

const Card = ({ imageUrl, title, description, isBig, toggleSize, tags }) => {
  const [isHovered, setIsHovered] = useState(false);

  useEffect(() => {
    window.scrollTo({ top: 0, behavior: "smooth" });
  }, []);

  // Merge tags into a single string separated by "/"
  const mergedTags = tags && tags.length > 1 ? tags.join(" / ") : tags?.[0];

  return (
    <div className={styles.cardsContainer}>
      <div
        className={`${styles.card} ${isBig ? styles.big : ""}`}
        style={{ backgroundImage: `url(${imageUrl})` }}
        onClick={toggleSize}
      >
        {mergedTags && (
          <div className={styles.tagsContainer}>
            <span className={styles.tag}>{mergedTags}</span>
          </div>
        )}
        {!isBig && <div className={styles.cardTitle}>{title}</div>}
        {isBig && (
          <div className={styles.cardContent}>
            <h3>{title}</h3>
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



import IntelligentAssist from './CardsData/IntelligentAssist.json';
import EmailEAR from './CardsData/EmailEAR.json';
import CaseIntelligence from './CardsData/CaseIntelligence.json';
import SmartRecruit from './CardsData/SmartRecruit.json';
import ClaimAssist from './CardsData/ClaimAssist.json';
import AssistantEV from './CardsData/AssistantEV.json';
import AutoWiseCompanion from './CardsData/AutoWiseCompanion.json';
import CitizenAdvisor from './CardsData/CitizenAdvisor.json';
import FinCompetitor from './CardsData/FinCompetitor.json';
import SignatureExtraction from './CardsData/SignatureExtraction.json';
import AiForce from './CardsData/AiForce.json';
import ApiCase from './CardsData/ApiCase.json';
import AmsSupport from './CardsData/AmsSupport.json';
import CodeGreat from './CardsData/CodeGreat.json';
import AaigApi from './CardsData/AaigApi.json';
import ResponsibleGen from './CardsData/ResponsibleGen.json';
import GraphData from './CardsData/GraphData.json';
import PredictiveAsset from './CardsData/PredictiveAsset.json';

import { images, fetchAssets } from './AssetImports';

// Mapping assets for a specific card
async function mapAssets(card) {
  const assets = await fetchAssets();
  
  // Sanitize the title to match the keys in urldata.json
  const assetKey = card.title.replace(/\s+/g, '');

  // Access the data for the specific asset
  const data = {
    solutionFlow: assets[`${assetKey}solutionFlow`] || null,
    techArchitecture: assets[`${assetKey}techArchitecture`] || null,
    description: assets[`${assetKey}description`] || null,
    benefits: assets[`${assetKey}benefits`] || null,
    adoption: assets[`${assetKey}adoption`] || null,
    demo: assets[`${assetKey}demo`] || null,
  };
  
  console.log(`Data for ${card.title}:`, data);

  return {
    ...card,
    imageUrl: images[card.imageUrl] || 'defaultImagePath.jpg', // Use a default image if the specified one is missing
    content: {
      ...card.content,
      solutionFlow: data.solutionFlow,
      demo: data.demo || null,
      techArchitecture: data.techArchitecture,
      description: data.description,
      benefits: data.benefits,
      adoption: data.adoption,
    },
  };
}

// Mapping cards data asynchronously
export async function getCardsData() {
  const cardsData = await Promise.all([
    mapAssets(IntelligentAssist),
    mapAssets(EmailEAR),
    mapAssets(CaseIntelligence),
    mapAssets(SmartRecruit),
    mapAssets(ClaimAssist),
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
  ]);

  return cardsData;
}


import React, { useState, useEffect, useRef } from "react";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faArrowRight, faArrowLeft, faPlay, faPause } from "@fortawesome/free-solid-svg-icons";
import MyCarousel from "./components/Carousel/MyCarousel";
import Cards from "./components/Cards/Cards";
import styles from "./App.module.css";
import { Link } from "react-router-dom";

const S3_VIDEO_URL = "https://aiml-convai.s3.amazonaws.com/portal-bg-video/BgVideos.webM";

const Home = ({
  cardsData,
  handleClickLeft,
  handleClickRight,
  currentIndex,
  bigIndex,
  toggleSize,
  cardsContainerRef,
  handleMouseEnter,
}) => {
  const [videoState, setVideoState] = useState("hidden");
  const [isPlaying, setIsPlaying] = useState(false);
  const videoRef = useRef(null);

  const handleScroll = () => {
    if (videoRef.current) {
      const videoPosition = videoRef.current.getBoundingClientRect().top;
      const triggerPoint = window.innerHeight / 2;

      if (videoPosition <= triggerPoint && videoState !== "big") {
        setVideoState("big");
        if (!isPlaying) {
          videoRef.current.play().catch(error => {
            console.error("Error during video playback:", error);
          });
          setIsPlaying(true);
        }
      } else if (videoPosition > triggerPoint + 100 && videoState !== "small") {
        setVideoState("small");
        if (isPlaying) {
          videoRef.current.pause();
          setIsPlaying(false);
        }
      }
    }
  };

  const debounce = (func, wait) => {
    let timeout;
    return (...args) => {
      clearTimeout(timeout);
      timeout = setTimeout(() => func.apply(this, args), wait);
    };
  };

  const handleDebouncedScroll = debounce(handleScroll, 100);

  const togglePlayPause = () => {
    const videoElement = videoRef.current;
    if (videoElement) {
      try {
        if (isPlaying) {
          videoElement.pause();
        } else {
          videoElement.play().catch(error => {
            console.error("Error during video playback:", error);
          });
        }
        setIsPlaying(!isPlaying);
      } catch (error) {
        console.error("Error during video playback:", error);
      }
    } else {
      console.error("Video element is not available or not properly referenced.");
    }
  };

  useEffect(() => {
    window.addEventListener("scroll", handleDebouncedScroll);

    return () => {
      window.removeEventListener("scroll", handleDebouncedScroll);
    };
  }, [handleDebouncedScroll]);

  return (
    <>
      <MyCarousel />
      <div className={`${styles.videoContainer} ${styles[videoState]}`}>
        <video
          className={styles.video}
          src={S3_VIDEO_URL}
          muted
          loop
          ref={videoRef}
          playsInline
          onClick={togglePlayPause} // Allow play/pause by clicking on video
          onCanPlay={() => videoRef.current.play()} // Ensure video plays when it can
        />
        <button
          className={`${styles.playPauseButton} ${!isPlaying ? "pulse" : ""}`}
          onClick={togglePlayPause}
        >
          <FontAwesomeIcon icon={isPlaying ? faPause : faPlay} />
        </button>
      </div>
      <div
        className={styles.cardsContainer}
        ref={cardsContainerRef}
        onMouseEnter={handleMouseEnter}
      >
        <div className={styles.viewAllContainer}>
          <Link to="/all-cards" className={styles.viewAllButton}>
            View All Solutions <FontAwesomeIcon icon={faArrowRight} className={styles.icon} />
          </Link>
        </div>
        <span className={`${styles.arrow} ${styles.leftArrow}`} onClick={handleClickLeft}>
          <FontAwesomeIcon icon={faArrowLeft} title="Previous" />
        </span>
        {cardsData.slice(currentIndex, currentIndex + 5).map((card, index) => {
          const actualIndex = currentIndex + index;
          return (
            <Cards
              key={index}
              imageUrl={card.imageUrl}
              title={card.title}
              description={card.description}
              isBig={actualIndex === bigIndex}
              toggleSize={() => toggleSize(actualIndex)}
            />
          );
        })}
        <span className={`${styles.arrow} ${styles.rightArrow}`} onClick={handleClickRight}>
          <FontAwesomeIcon icon={faArrowRight} title="Next" />
        </span>
      </div>
    </>
  );
};

export default Home;









