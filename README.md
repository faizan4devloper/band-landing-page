my facing a problem with the percentage api after api hit then data will null and then i check the api gateway then its totaaly working fine and then i update the code and then it working and from starting its it null pls fixed
  
  import React, { useState, useEffect } from "react";
  import axios from "axios";
  import Modal from "react-modal";
  import styles from "./MainContent.module.css";
  import { useNavigate } from "react-router-dom";
  import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
  import { faSync, faChevronRight } from '@fortawesome/free-solid-svg-icons';
  
  import DocumentPreview from "./DocumentPreview";
  import ExtractedContent from "./ExtractedContent";
  import ClaimClassification from "./ClaimClassification";
  import ClaimProcessingStatus from "./ClaimProcessingStatus";
  
  const MainContent = ({ message, rows, clid, setRows, staticPreviewUrl, selectedPolicy, uploadedFileName }) => {
    const navigate = useNavigate();
    const [loading, setLoading] = useState(false);

    const [data, setData] = useState(null);
    const [percentage, setPercentage] = useState(null);
    const [isLoading, setIsLoading] = useState(false);
      const [embeddingStatus, setEmbeddingStatus] = useState(null);
    
  
    // Existing useEffects (fetchPercentage and data loading)
    useEffect(() => {
      if (rows.length > 0) {
        handleReload(rows[0].recNum);
      }
    }, [rows]);
    
    const handleDocumentEmbedQueue = async () => {
    if (rows.length === 0) return;

    const currentClaimId = rows[0].recNum;
    const claimFormPath = data?.total_extracted_data 
      ? JSON.parse(data?.total_extracted_data).CLAIM_FORM_DETAILS?.CLAIM_FORM_PATH 
      : null;

    if (!claimFormPath) {
      console.warn('No claim form path found');
      return;
    }

    try {
      const payload = {
        claimid: currentClaimId,
        recnumber: currentClaimId,
        claimformpath: claimFormPath
      };

      const response = await axios.post(
        "https://docembedqueue",
        payload,
      
        { 
          headers: { 
            "Content-Type": "application/json" 
          } 
        }
      );

      console.log('Document Embed Queue Response:', response);
      
      // Optional: Set embedding status
      setEmbeddingStatus(response.data);
    } catch (error) {
      console.error("Failed to call document embed queue:", error);
      setEmbeddingStatus(null);
    }
  };

  // Trigger document embedding when data is loaded
  useEffect(() => {
    if (data && data.total_extracted_data) {
      handleDocumentEmbedQueue();
    }
  }, [data]);

  
useEffect(() => {
  const fetchPercentage = async () => {
    // Check if rows exist and have at least one item
    if (rows.length > 0) {
      const currentClaimId = rows[0].recNum; // Use the actual claim ID from rows
      
      setIsLoading(true);
      try {
        const response = await axios.post(
          "https://percentage",
          { claimid: currentClaimId }, // Use dynamic claim ID
          { 
            headers: { 
              "Content-Type": "application/json" 
            } 
          }
        );
        
        console.log('Response Percentage:', response);

        // Check if response has data and body
        if (response.data && response.data.body) {
          let percentageValue = response.data.body.empty_key_perc 
            ? parseFloat(response.data.body.empty_key_perc) 
            : 0;

          setPercentage(percentageValue);
        } else {
          console.warn('No percentage data found in response');
          setPercentage(0);
        }
      } catch (error) {
        console.error("Error fetching percentage:", error);
        
        // If there's an error, check the full error response
        if (error.response) {
          console.error('Error response:', error.response);
        }
        
        setPercentage(0);
      } finally {
        setIsLoading(false);
      }
    }
  };

  fetchPercentage();
}, [rows]); 
  
    const handleVerify = (clid, formtype, summary, selectedPolicy) => {
      if (formtype === 'CANCER') {
        selectedPolicy = 'PS391481';
      } else if (formtype === 'HEART') {
        selectedPolicy = 'PS672908';
      } else {
        selectedPolicy = 'PS672908';
      }
  
      navigate('/verify', {
        state: { clid, summary, selectedPolicy },
      });
    };
  
    const handleReload = async (recNum) => {
      setLoading(true);
      try {
        const payload = {
          tasktype: "FETCH_SINGLE_ACT_CLAIM",
          claimid: recNum,
        };
  
        const response = await axios.post(
          "https:all", 
          payload, 
          { headers: { "Content-Type": "application/json" } }
        );
        
        console.log('Extraction:', response)
        
        setData(response.data.allclaimactdata);
      } catch (error) {
        console.error("Failed to fetch data for RecNum:", recNum, error);
      } finally {
        setLoading(false);
      }
    };
  
    return (
      <div className={styles.mainContentWrapper}>
        {rows.length > 0 && (
          <div className={styles.claimIdDisplay}>
            <h3>Claim ID: {rows[0].recNum}</h3>
          </div>
        )}
        
        <div className={styles.mainContentGrid}>
          <div className={styles.leftColumn}>
            <div className={styles.claimClassificationContainer}>
              <ClaimClassification data={data} uploadedFileName={uploadedFileName}/>
            </div>
            <div className={styles.documentPreviewContainer}>
              <DocumentPreview staticPreviewUrl={staticPreviewUrl} />
            </div>
            
            
          </div>
          
          
  
          <div className={styles.rightColumn}>
            <div className={styles.percentageSection}>
            <ClaimProcessingStatus 
              percentage={percentage} 
              isLoading={isLoading} 
            />
          </div>
          
            <div className={styles.extractedContentContainer}>
              <ExtractedContent 
                data={data} 
                loading={loading} 
                rows={rows} 
                handleReload={handleReload}
              />
            </div>
            
            <div className={styles.verifyButtonContainer}>
              <button
                className={styles.verifyButton}
                onClick={() =>
                  handleVerify(
                    clid,
                    data?.total_extracted_data
                      ? JSON.parse(data?.total_extracted_data).CLAIM_FORM_DETAILS?.CLAIM_FORM_TYPE
                      : "No Claim Form Type",
                    data?.total_extracted_data
                      ? JSON.parse(data?.total_extracted_data).CLAIM_FORM_DETAILS?.CLAIM_FORM_DETAILED_SUMMARY
                      : "No Summary Available",
                    selectedPolicy
                  )
                }
                disabled={!rows.length}
              >
                Verify Claim <FontAwesomeIcon icon={faChevronRight}/>
              </button>
            </div>
          </div>
        </div>
      </div>
    );
  };
  
  export default MainContent;


Response body
{"statusCode": 200, "headers": {"Access-Control-Allow-Origin": "*", "Access-Control-Allow-Headers": "*", "Access-Control-Allow-Methods": "*"}, "body": {"claimid": "CL741795", "empty_keys": [], "empty_keys_count": 0, "total_keys": 28, "empty_key_perc": "0%"}}
