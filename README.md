import React, { useState } from "react";
import axios from "axios";
import styles from "./MainContent.module.css";

const MainContent = () => {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(false);

  const handleReload = async () => {
    setLoading(true);
    try {
      const payload = {
        tasktype: "FETCH_SINGLE_ACT_CLAIM",
        claimid: "CL928697",
      };

      const response = await axios.post(`dummy`, payload, {
        headers: { "Content-Type": "application/json" },
      });

      console.log(response.data);
      setData(response.data.allclaimactdata);
    } catch (error) {
      console.error("Error fetching data:", error);
    } finally {
      setLoading(false);
    }
  };

  const renderContent = (content) => {
    if (typeof content === "object") {
      return (
        <div className={styles.nestedContent}>
          {Object.entries(content).map(([key, value]) => (
            <div key={key}>
              <h3 className={styles.subheading}>{key}</h3>
              {renderContent(value)}
            </div>
          ))}
        </div>
      );
    } else {
      return <p className={styles.paragraph}>{content}</p>;
    }
  };

  return (
    <div className={styles.mainContent}>
      <div className={styles.extractContentSection}>
        <h2 className={styles.heading}>Extract Content</h2>
        {loading ? (
          <p>Loading...</p>
        ) : data ? (
          <div className={styles.contentContainer}>
            <h3>Claim ID: {data.claimid}</h3>
            <div>
              <h3>Document Name:</h3>
              <ul className={styles.documentList}>
                {data.document_name.map((doc, index) => (
                  <li key={index}>{doc.S}</li>
                ))}
              </ul>
            </div>
            <div>
              <h3>Total Extracted Data:</h3>
              {renderContent(JSON.parse(data.total_extracted_data))}
            </div>
          </div>
        ) : (
          <p>No data available</p>
        )}

        <button
          className={styles.reloadButton}
          onClick={handleReload}
          disabled={loading}
        >
          Reload Data
        </button>
      </div>
    </div>
  );
};

export default MainContent;
