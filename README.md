import React, { useState, useEffect } from "react";
import { ClipLoader } from "react-spinners"; // Import spinner
import styles from "./ClaimClassification.module.css";

const ClaimClassification = ({ data, uploadedFileName }) => {
  const [documentName, setDocumentName] = useState(null);
  const [classification, setClassification] = useState(null);
  const [isValid, setIsValid] = useState(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    if (data) {
      try {
        setDocumentName(
          data.CLAIM_FORM_DETAILS?.DOCUMENT_NAME ||
            data.UPLOADED_DOCUMENT_NAME ||
            uploadedFileName ||
            "Unnamed Document"
        );

        setClassification(data.CLAIM_FORM_DETAILS?.CLAIM_FORM_TYPE || "Unknown");

        // Check if DETAILS_OF_ILLNESS exists and has content
        const hasIllnessDetails =
          data.DETAILS_OF_ILLNESS && Object.keys(data.DETAILS_OF_ILLNESS).length > 0;
        setIsValid(hasIllnessDetails ? "Yes" : "No");

        setLoading(false); // Stop loading once data is available
      } catch (error) {
        console.error("Error processing classification data:", error);
        setDocumentName("Error Loading Document");
        setClassification("Error");
        setIsValid("Error");
        setLoading(false);
      }
    }
  }, [data, uploadedFileName]);

  return (
    <div className={styles.claimClassificationContainer}>
      <h4>Claim Classification</h4>
      <table className={styles.classificationTable}>
        <thead>
          <tr>
            <th>Document Name</th>
            <th>Classification</th>
            <th>Is Valid?</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td>
              {loading ? <ClipLoader size={20} color="#2d3748" /> : documentName}
            </td>
            <td>
              {loading ? (
                <ClipLoader size={20} color="#2d3748" />
              ) : (
                <span
                  className={`${styles.classificationTag} ${
                    classification?.toLowerCase() === "cancer"
                      ? styles.cancerTag
                      : classification?.toLowerCase() === "heart"
                      ? styles.heartTag
                      : styles.defaultTag
                  }`}
                >
                  {classification}
                </span>
              )}
            </td>
            <td>
              {loading ? (
                <ClipLoader size={20} color="#2d3748" />
              ) : (
                <span
                  className={`${styles.validTag} ${
                    isValid === "Yes" ? styles.validYes : styles.validNo
                  }`}
                >
                  {isValid || "N/A"}
                </span>
              )}
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  );
};

export default ClaimClassification;


.classificationTable td {
  display: flex;
  align-items: center;
  justify-content: center;
  min-height: 40px;
}
