useEffect(() => {
  const fetchHistoricClaimDetails = async () => {
    if (!claimType) return;

    setLoading(true);
    try {
      const response = await axios.post(
        'https://history',
        { claimtype: claimType }
      );

      // Log the raw response for debugging
      console.log('Raw Response:', response.data);

      let parsedBody;
      
      try {
        // Check if response.data.body is a string
        if (typeof response.data.body === 'string') {
          parsedBody = JSON.parse(response.data.body);
        } else {
          parsedBody = response.data.body;
        }

        // If parsedBody is still a string, try parsing again (handles double-encoded JSON)
        if (typeof parsedBody === 'string') {
          parsedBody = JSON.parse(parsedBody);
        }
      } catch (error) {
        console.error('JSON Parsing Error:', error);
        throw new Error('Invalid data format');
      }

      // Validate parsed data
      if (!parsedBody || typeof parsedBody !== 'object') {
        throw new Error('Invalid data format');
      }

      console.log('Parsed Historic Data:', parsedBody);
      setHistoricData(parsedBody);
      setLoading(false);
    } catch (err) {
      console.error('Error details:', {
        message: err.message,
        response: err.response ? err.response.data : 'No response',
        stack: err.stack
      });
      setError(err.message);
      setLoading(false);
    }
  };

  fetchHistoricClaimDetails();
}, [claimType]);






i provide you the data and also the error this error is coming sometemimes pls correct it

Raw Response: {statusCode: 200, headers: {…}, body: '"{\\n    \\"Approver1_Focuses_On\\": [],\\n    \\"Appro… leading to a positive customer experience.\\"\\n}"'}body: "\"{\\n    \\\"Approver1_Focuses_On\\\": [],\\n    \\\"Approver2_Focuses_On\\\": [\\n        \\\"Wet signature on the form\\\",\\n        \\\"Clarification on coverage limits and policy documents\\\",\\n        \\\"Accurate diagnosis date\\\"\\n    ],\\n    \\\"Additional_Information\\\": [\\n        \\\"Wet signature on the claim form (as electronic signatures are not accepted)\\\",\\n        \\\"Complete policy documents to verify coverage limits and ensure accurate claim payout\\\",\\n        \\\"Confirmation of the exact diagnosis date to process the claim accurately\\\"\\n    ],\\n    \\\"Recommendations\\\": \\\"Based on the claim history comments, it appears that Approver2 is focused on ensuring accurate documentation, policy details, and verification of key claim information before approving the claims. The recommendations for similar claims would be:\\n\\n1. Clearly communicate the requirement for a wet signature on the claim form, as electronic signatures are not accepted.\\n2. Request the claimant to provide the complete policy documents, including any relevant riders or endorsements, to enable a thorough review of the coverage limits and ensure the correct claim payout.\\n3. Confirm the exact date of diagnosis with the claimant and obtain updated medical reports to verify the information.\\n\\nBy addressing these key areas upfront, the claims approval process can be streamlined, and any potential issues can be resolved efficiently, leading to a positive customer experience.\\\"\\n}\""headers: {Access-Control-Allow-Origin: '*', Access-Control-Allow-Headers: '*', Access-Control-Allow-Methods: '*'}statusCode: 200[[Prototype]]: Object
Insights.js:55 


Error details: {message: 'Invalid data format', response: 'No response', stack: 'Error: Invalid data format\n    at fetchHistoricCla…om/main.d7581fb4a5664d36ac90.hot-update.js:70:17)'}





import React, { useState, useEffect } from 'react';
import axios from 'axios';
import styles from './Insights.module.css';
import { BeatLoader } from 'react-spinners';

import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faLightbulb, faChartLine, faCheckCircle } from '@fortawesome/free-solid-svg-icons';

const Insights = ({ claimType }) => {
  const [historicData, setHistoricData] = useState(null);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState(null);

  useEffect(() => {
    const fetchHistoricClaimDetails = async () => {
      if (!claimType) return;

      setLoading(true);
      try {
        const response = await axios.post(
          'https://history',
          { claimtype: claimType }
        );

        // Log the raw response for debugging
        console.log('Raw Response:', response.data);

        // Try different parsing approaches
        let parsedBody;
        try {
          // First, try parsing as nested JSON
          parsedBody = JSON.parse(JSON.parse(response.data.body));
        } catch (nestedParseError) {
          try {
            // If nested parsing fails, try direct parsing
            parsedBody = JSON.parse(response.data.body);
          } catch (directParseError) {
            // If both fail, parse the body directly
            parsedBody = response.data.body;
          }
        }

        // Validate parsed data
        if (!parsedBody || typeof parsedBody !== 'object') {
          throw new Error('Invalid data format');
        }

        // Log parsed data for verification
        console.log('Parsed Historic Data:', parsedBody);

        setHistoricData(parsedBody);
        setLoading(false);
      } catch (err) {
        console.error('Error details:', {
          message: err.message,
          response: err.response ? err.response.data : 'No response',
          stack: err.stack
        });
        setError(err.message);
        setLoading(false);
      }
    };

    fetchHistoricClaimDetails();
  }, [claimType]);

  // Rest of the component remains the same...

  // Add more robust rendering with default values
  if (!historicData) {
    return (
      <div className={styles.noDataContainer}>
<BeatLoader 
              color="#0f5fdc" 
              // loading={loadingLLM} 
              size={15} 
            />      </div>
    );
  }

  return (
<div className={styles.insightsContainer}>

      {/* Approver Insights Section */}
      <div className={styles.insightsSection}>
        <div className={styles.insightsGrid}>
          {/* Approver 1 Focus */}
          <div className={styles.insightsCard}>
            <ul>
              {historicData.Approver1_Focuses_On?.map((focus, index) => (
                <li key={index}>{focus}</li>
              ))}
            </ul>
          </div>

          {/* Approver 2 Focus */}
          <div className={styles.insightsCard}>
            <ul>
              {historicData.Approver2_Focuses_On?.map((focus, index) => (
                <li key={index}>{focus}</li>
              ))}
            </ul>
          </div>
        </div>
      </div>

      {/* Additional Information Section */}
      <div className={styles.insightsSection}>
        <h4>Additional Information</h4>
        <ul>
          {historicData.Additional_Information?.map((info, index) => (
            <li key={index}>{info}</li>
          ))}
        </ul>
      </div>

      {/* Recommendations Section */}
     
    </div>  );
};

export default Insights;
