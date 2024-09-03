import React, { useState, useEffect } from "react";
import { Link } from "react-router-dom";
import styles from "./Cards.module.css";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faArrowRight } from "@fortawesome/free-solid-svg-icons";

const Card = ({ imageUrl, title, description, isBig, toggleSize, tags, hasData }) => {
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
              <div className={styles.noDataMessage}>No Data Available</div>
            )}
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

  // Determine if data is available for the card
  const hasData = !!(
    data.solutionFlow ||
    data.techArchitecture ||
    data.description ||
    data.benefits ||
    data.adoption ||
    data.demo
  );

  console.log(`Data for ${card.title}:`, data);

  return {
    ...card,
    imageUrl: images[card.imageUrl] || 'defaultImagePath.jpg', // Use a default image if the specified one is missing
    content: {
      ...card.content,
      solutionFlow: data.solutionFlow,
      demo: data.demo,
      techArchitecture: data.techArchitecture,
      description: data.description,
      benefits: data.benefits,
      adoption: data.adoption,
    },
    hasData, // Add hasData property
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

export default getCardsData;
