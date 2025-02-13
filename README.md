import React, { useState } from "react";
import axios from "axios";
import Sidebar from "./Sidebar/Sidebar";
import MainContent from "./MainContent/MainContent";
import Verify from "./Verify/Verify";
import styles from "./NewClaimPage.module.css";
import BreadCrumbs from "./../BreadCrumbs/BreadCrumbs";
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faBars, faTimes } from '@fortawesome/free-solid-svg-icons';


  import { useClaimContext } from './../Context/ClaimContext';


const NewClaimPage = () => {
  const [isSidebarVisible, setIsSidebarVisible] = useState(false);
  
  const breadcrumbItems = [
    { label: 'Claims', link: '/claims' },
    { label: 'New Claim', link: '/new-claim' }
  ];

  const {
    file, setFile,
    message, setMessage,
    uploading, setUploading,
    uploadedFileName, setUploadedFileName,
    rows, setRows,
    isUploaded, setIsUploaded,
    previewUrl, setPreviewUrl,
    selectedPolicy, setSelectedPolicy
  } = useClaimContext();

  const handleFileChange = (e) => {
    const selectedFile = e.target.files[0];
    if (selectedFile) {
      const sanitizedFileName = selectedFile.name.replace(/\s+/g, "_");
      setFile(selectedFile);
      setUploadedFileName(sanitizedFileName);
      setPreviewUrl(URL.createObjectURL(selectedFile));
      setMessage("");
    }
  };

  const handlePolicyChange = (policy) => {
    setSelectedPolicy(policy);
  };
  
    const toggleSidebar = () => {
    setIsSidebarVisible(!isSidebarVisible);
  };


  const handleUpload = async () => {
    if (!file) {
      setMessage("Please select a file to upload.");
      return;
    }
    setUploading(true);
    try {
      const sanitizedFileName = file.name.replace(/\s+/g, "_");
      const response = await axios.post(
        "https:///presignUrl",
        {
          payload: { filename: sanitizedFileName, filetype: "CL" },
        }
      );
      
      console.log("Presign url", response.data)

      const { presignedUrl, key, recNum } = response.data;
      
      console.log("**Response Api",response.data)

      await axios.put(presignedUrl, file, {
        headers: { "Content-Type": file.type },
      });

      const sqsPayload = {
        claimid: recNum,
        s3filename: key,
        // tasktype: "SEND_TO_QUEUE", 
  //       claimid : "CL004681",
  // s3filename: "claimassistv2/claimforms/CL004681/Case_CI_Cancer_3.1.pdf"
      };

    const sqsResponse=  await axios.post(
        "https://sqsqueue",
        sqsPayload,
        { headers: { "Content-Type": "application/json" } }
      );
      
      console.log("SQS API Response:", sqsResponse.data);

      setRows((prevRows) => [
        ...prevRows,
        {
          recNum,
          policyid: "",
          type: "",
          summary: "",
          previewLink: presignedUrl,
         policy: selectedPolicy, // Include the selected policy

          status: "Pending",
        },
      ]);

      setIsUploaded(true);
    } catch (error) {
      setMessage("Upload failed: " + error.message);
    } finally {
      setUploading(false);
    }
  };
  return (
    <div>
          
<BreadCrumbs 
        items={breadcrumbItems} 
        // Optional: Add custom styling
        className={styles.customBreadcrumbs}
        style={{
          // Override default styles if needed
          backgroundColor: '#ffffff',
          borderBottom: '1px solid #e1e4e8'
        }}
      />

    <div className={styles.container}>
             <div className={`${styles.sidebar} ${!isSidebarVisible ? styles.sidebarHidden : ''}`}>
          <Sidebar
            onFileChange={handleFileChange}
            onUpload={handleUpload}
            uploading={uploading}
            onPolicyChange={handlePolicyChange}
            toggleSidebar={toggleSidebar}
            
          />
        </div>

      <div className={styles.mainContent}>
        {!isSidebarVisible && (
        <button className={styles.toggleButton} onClick={toggleSidebar}>
          <FontAwesomeIcon icon={faBars} />
        </button>
      )}

        {isUploaded ? (
          <MainContent 
            message={message} 
            selectedPolicy={selectedPolicy}
            rows={rows} 
            clid={rows[0].recNum}
            setRows={setRows} 
            staticPreviewUrl={previewUrl} 
            uploadedFileName={uploadedFileName}
          />
        ) : (
          <p className={styles.infoMessage}>
            Please upload a document to view the data.
          </p>
        )}
      </div>
    </div>
    </div>
  );
};

export default NewClaimPage;


.container {
  display: flex;
  height: 100vh;
  width: 100%;
  overflow: hidden;
}

.sidebar {
  flex: 0 0 300px;
  height: 100%;
  padding: 20px;
  box-sizing: border-box;
  transition: all 0.3s ease-in-out;
  transform: translateX(0);
}

.sidebarHidden {
  flex: 0 0 0;
  transform: translateX(-100%);
  opacity: 0;
  padding: 0;
  overflow: hidden;
}

.mainContent {
  flex: 1;
  height: 100%;
  background-color: #fff;
  overflow-y: auto;
  padding: 20px;
  box-sizing: border-box;
  /*transition: all 0.3s ease-in-out;*/
}

.toggleButton {
   margin-bottom: 8px;
    z-index: 1000;
    background: linear-gradient(90deg, #0f5fdc, #7ca2e1);
    color: white;
    border: none;
    border-radius: 7%;
    width: 40px;
    height: 31px;
    display: flex
;
    align-items: center;
    justify-content: center;
    cursor: pointer;
    box-shadow: 0 2px 5px rgba(0, 0, 0, 0.2);
    transition: transform 0.3s ease;
}

.toggleButton:hover {
  transform: scale(1.1);
}


  .document-preview-container {
    background-color: #f4f6f9;
    border-radius: 12px;
    padding: 20px;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  }

  .upload-prompt-container {
    display: flex;
    justify-content: center;
    align-items: center;
    min-height: 300px;
  }

  .upload-prompt-card {
    text-align: center;
    background-color: #ffffff;
    border-radius: 16px;
    padding: 40px;
    box-shadow: 0 10px 25px rgba(0, 0, 0, 0.1);
    max-width: 500px;
    width: 100%;
  }

  .upload-icon {
    width: 80px;
    height: 80px;
    color: #3b82f6;
    margin-bottom: 20px;
    opacity: 0.7;
  }

  .upload-message {
    font-size: 18px;
    color: #2c3e50;
    margin-bottom: 10px;
    font-weight: 600;
  }

  .upload-hint {
    color: #718096;
    font-size: 14px;
  }

  .animate-fade-in {
    animation: fadeIn 0.5s ease-in-out;
  }
  
  .infoMessage{
    margin:150px auto;
    text-align: center;
  }

  @keyframes fadeIn {
    from { opacity: 0; transform: translateY(20px); }
    to { opacity: 1; transform: translateY(0); }
  }




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
        "https:docembedqueue",
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
    if (rows.length === 0 || !rows[0]?.recNum) return;

    const currentClaimId = rows[0].recNum;
    setIsLoading(true);
    try {
      const response = await axios.post(
        "https://percentage",
        { claimid: currentClaimId },
        { headers: { "Content-Type": "application/json" } }
      );

      if (response.data && response.data.body) {
        let percentageValue = response.data.body.empty_key_perc 
          ? parseFloat(response.data.body.empty_key_perc.replace('%', ''))
          : 0;
          
        setPercentage(percentageValue);
      }
    } catch (error) {
      setPercentage(0);  // Set to 0 if error occurs
    } finally {
      setIsLoading(false);
    }
  };

  fetchPercentage();
}, [rows, data]);

    const handleVerify = (clid, formtype, summary, selectedPolicy) => {
      if (formtype === 'CANCER') {
        selectedPolicy = 'PS391481';
      } else if (formtype === 'HEART') {
        selectedPolicy = 'PS672908';
      } else {
        selectedPolicy = 'PS672908';
      }
  
      navigate('/verify', {
        state: { clid, summary, selectedPolicy, percentage },
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
          "https://devstage/all", 
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
<ClaimClassification
  data={
    data?.total_extracted_data
      ? JSON.parse(data.total_extracted_data)
      : null
  }
  uploadedFileName={uploadedFileName}
/>
            </div>
            <div className={styles.documentPreviewContainer}>
              <DocumentPreview staticPreviewUrl={staticPreviewUrl} />
            </div>
            
            
          </div>
          
          
  
          <div className={styles.rightColumn}>
            <div className={styles.percentageSection}>
  {isLoading ? (
    <p>Loading percentage...</p>  // Show loading text or a spinner
  ) : (
    <ClaimProcessingStatus 
      percentage={percentage} 
      isLoading={isLoading} 
    />
  )}
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


  /* MainContent.module.css */
/* Design System Variables */
:root {
  --primary-color: #2c3e50;
  --secondary-color: #3498db;
  --background-light: #f8f9fa;
  --border-color: #ecf0f1;
  --text-dark: #2c3e50;
  --text-light: #ffffff;
  --spacing-unit: 1rem;
  --border-radius: 8px;
  --box-shadow: 0 2px 15px rgba(0, 0, 0, 0.1);
}

/*.mainContentWrapper {*/
/*  padding: calc(var(--spacing-unit) * 2);*/
/*  height: 100vh;*/
/*  overflow-y: auto;*/
/*}*/

.claimIdDisplay {
  background-color: var(--primary-color);
  color: var(--text-light);
  padding: var(--spacing-unit);
  border-radius: var(--border-radius);
  margin-bottom: var(--spacing-unit);
  box-shadow: var(--box-shadow);
}

.mainContentGrid {
  display: grid;
  grid-template-columns: 1fr 1.5fr;
  gap: calc(var(--spacing-unit) * 2);
  height: calc(100% - 60px);
}

.leftColumn, .rightColumn {
  display: flex;
  flex-direction: column;
  gap: var(--spacing-unit);
}

/* Card Layout Template */
.cardContainer {
  background: var(--text-light);
  border-radius: var(--border-radius);
  border: 1px solid var(--border-color);
  box-shadow: var(--box-shadow);
  padding: var(--spacing-unit);
  transition: transform 0.2s ease;
}

.cardContainer:hover {
  transform: translateY(-2px);
}

/* Specific Component Containers */
.claimClassificationContainer {
  flex: 0 1 auto;
}

.documentPreviewContainer {
  flex: 1 1 50%;
  min-height: 400px;
}

.percentageSection {
  height: 120px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.extractedContentContainer {
  flex: 1;
  min-height: 400px;
}

/* Verify Button Styling */
.verifyButtonContainer {
  margin-top: auto;
  padding-top: var(--spacing-unit);
}

.verifyButton {
  background-color: var(--secondary-color);
  color: var(--text-light);
  border: none;
  padding: calc(var(--spacing-unit) * 0.75) calc(var(--spacing-unit) * 2);
  border-radius: var(--border-radius);
  cursor: pointer;
  width: 100%;
  font-size: 1.1rem;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: var(--spacing-unit);
  transition: all 0.3s ease;
}

.verifyButton:hover {
  background-color: #2980b9;
  transform: scale(1.02);
}

.verifyButton:disabled {
  background-color: #95a5a6;
  cursor: not-allowed;
  opacity: 0.7;
}

/* Loading States */
.loadingText {
  color: var(--secondary-color);
  display: flex;
  align-items: center;
  gap: var(--spacing-unit);
}

/* Responsive Design */
@media (max-width: 1200px) {
  .mainContentGrid {
    grid-template-columns: 1fr;
    height: auto;
  }

  .leftColumn, .rightColumn {
    min-height: auto;
  }

  .documentPreviewContainer {
    min-height: 300px;
  }
}


