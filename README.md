// src/Dashboard.js
import React, { useEffect, useState } from 'react';
import styles from './Dashboard.module.css';
import claimData from './claimData.json'; // Import the JSON data

const Dashboard = () => {
  const [data, setData] = useState({});
  const [docData, setDocData] = useState({});

  useEffect(() => {
    // Set the data from the imported JSON
    setData(claimData['claimassist-history-lambda']);
    setDocData(claimData['claimassist-docextract-lambda'].Output_Response);
  }, []);

  return (
    <div className={styles.dashboard}>
      <h1>Claim Assist Dashboard</h1>

      <div className={styles.section}>
        <h2>Claim History</h2>
        {Object.entries(data).map(([key, values]) => {
          if (Array.isArray(values)) {
            return (
              <div key={key}>
                <h3>{key.replace(/_/g, ' ')}</h3>
                <ul>
                  {values.map((item, index) => (
                    <li key={index}>{item}</li>
                  ))}
                </ul>
              </div>
            );
          }
          return null; // Return null for non-array values
        })}
      </div>

      <div className={styles.section}>
        <h2>Document Extraction</h2>
        {Object.entries(docData.ExtractedData).map(([formType, details]) => (
          <div key={formType}>
            <h3>{formType.replace(/_/g, ' ')}</h3>
            {Object.entries(details).map(([detailKey, detailValue]) => (
              <p key={detailKey}>
                {detailKey.replace(/_/g, ' ')}: {detailValue}
              </p>
            ))}
          </div>
        ))}
      </div>

      <div className={styles.section}>
        <h2>Suggested Actions</h2>
        <ul>
          {data.Suggested_Action_Items && data.Suggested_Action_Items.map((action, index) => (
            <li key={index}>{action}</li>
          ))}
        </ul>
      </div>

      <div className={styles.section}>
        <h2>Detailed Summary</h2>
        <p>{data.Detailed_Summary}</p>
      </div>
    </div>
  );
};

export default Dashboard;
