in MainContent i have api in that i have the CLAIM_FORM_DETAILED_SUMMARY and i want this display in Verify.js left side window on verify leftPanel

import React, { useState } from "react";
import axios from "axios";
import Modal from "react-modal";
import styles from "./MainContent.module.css";
import { useNavigate } from "react-router-dom";

import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faSync } from '@fortawesome/free-solid-svg-icons';


const MainContent = ({ message, rows, setRows, staticPreviewUrl }) => {
  const [isModalOpen, setIsModalOpen] = useState(false);
    const navigate = useNavigate();

  const [modalContent, setModalContent] = useState(null);
  const [loading, setLoading] = useState(false);
  const [data, setData] = useState(null); // To store fetched data

 const handleVerify = () =>{
    navigate("/verify");
  }


  const handleReload = async (recNum) => {
    setLoading(true);
    try {
      const payload = {
        tasktype: "FETCH_SINGLE_ACT_CLAIM",
        claimid: recNum,
      };

      const response = await axios.post(`dummy`, payload, {
        headers: { "Content-Type": "application/json" },
      });
      
      
      console.log("Api", response.data)

      setData(response.data.allclaimactdata); // Store fetched data
    } catch (error) {
      console.error("Failed to fetch data for RecNum:", recNum, error);
    } finally {
      setLoading(false);
    }
  };

  const renderReadableContent = (data) => {
    if (!data) return <p>No data available</p>;

    const parsedData = JSON.parse(data);

    return (
      <div className={styles.readableContent}>
        <h4>Claim Form Details</h4>
        <p><strong>Type:</strong> {parsedData.CLAIM_FORM_DETAILS?.CLAIM_FORM_TYPE}</p>

        <h4>Clinical Abstract Application</h4>
        <p><strong>Name of Patient:</strong> {parsedData.CLINICAL_ABSTRACT_APPLICATION?.NAME_OF_PATIENT}</p>
        <p><strong>NRIC/FIN/BC:</strong> {parsedData.CLINICAL_ABSTRACT_APPLICATION?.NRIC_FIN_BC}</p>
        <p><strong>Address:</strong> {parsedData.CLINICAL_ABSTRACT_APPLICATION?.ADDRESS}</p>
        <p><strong>Date:</strong> {parsedData.CLINICAL_ABSTRACT_APPLICATION?.DATE}</p>

        <h4>Claimant Statement</h4>
        <h5>Policy Details</h5>
        <p><strong>Policy Numbers:</strong> {parsedData.CLAIMANT_STATEMENT?.POLICY_DETAILS?.POLICY_NUMBERS}</p>
        <p><strong>Type of Claim:</strong> {parsedData.CLAIMANT_STATEMENT?.POLICY_DETAILS?.TYPE_OF_CLAIM}</p>

        <h5>Details of Life Assured</h5>
        <p><strong>Name:</strong> {parsedData.CLAIMANT_STATEMENT?.DETAILS_OF_LIFE_ASSURED?.NAME_OF_LIFE_ASSURED}</p>
        <p><strong>Gender:</strong> {parsedData.CLAIMANT_STATEMENT?.DETAILS_OF_LIFE_ASSURED?.GENDER}</p>
        <p><strong>Marital Status:</strong> {parsedData.CLAIMANT_STATEMENT?.DETAILS_OF_LIFE_ASSURED?.MARITAL_STATUS}</p>
      </div>
    );
  };

return (
  <div className={styles.mainContent}>
    {/* Left: Document Preview */}
    
 <div className={styles.previewSection}>
        <h3>Document Preview</h3>
        {staticPreviewUrl ? (
          <iframe
            src={staticPreviewUrl}
            title="Static Document Preview"
            className={styles.documentPreviewIframe}
          ></iframe>
        ) : (
          <p>No document available for preview.</p>
        )}
      </div>

    {/* Right: Extracted Content */}
    <div className={styles.extractContentSection}>
      <h3>Extracted Content</h3>
      {loading ? (
        <p>Loading...</p>
      ) : (
        renderReadableContent(data?.total_extracted_data)
      )}
      
     
{rows.map((row, index) => (
          <div key={index}>
            <button
              className={styles.reloadButton}
              onClick={() => handleReload(row.recNum)} // Use dynamic recNum here
              disabled={loading}
            >
              <FontAwesomeIcon
                icon={faSync}
                spin={loading} // Spin when loading is true
                className={styles.icon}
              />
              {!loading && " Reload"} {/* Show text only when not loading */}
            </button>
          </div>
        ))}
</div>

    {/* Centered Verify Button */}
    <div className={styles.verifyButtonContainer}>
      <button
        className={styles.verifyButton}
        onClick={handleVerify}
      >
        Verify
      </button>
    </div>

    {/* Modal for Document Preview */}
    <Modal
      isOpen={isModalOpen}
      onRequestClose={() => setModalContent(null)}
      className={styles.modal}
      overlayClassName={styles.modalOverlay}
      ariaHideApp={false}
    >
      <div className={styles.modalContent}>
        <button
          className={styles.closeButton}
          onClick={() => setModalContent(null)}
        >
          Close
        </button>
        {modalContent ? (
          <iframe
            src={modalContent}
            title="Preview"
            className={styles.modalIframe}
          ></iframe>
        ) : (
          <p>No preview available</p>
        )}
      </div>
    </Modal>
  </div>
);
};

export default MainContent;



otal_extracted_data
: 
"{  \"CLAIM_FORM_DETAILS\": {    \"CLAIM_FORM_DETAILED_SUMMARY\": \"The claim form is for a cancer claim. It appears that the life assured, E G L H, was diagnosed with a malignant neoplasm of the sigmoid colon on 9th December 2021. The life assured first experienced symptoms of abdominal pain on 8th December 2021 and consulted a doctor on 9th December 2021, who then referred her to a specialist. The life assured underwent various tests and was hospitalized at Gleneagles Hospital from 9th December 2021 to 18th December 2021 for a minimally invasive surgery. After the surgery, the life assured was referred to another doctor for adjuvant treatment. The claim form provides detailed information about the life assured's medical history, the course of treatment, and the test results that confirm the diagnosis of cancer. The life assured is also claiming hospitalization benefits from another insurer, NTUC Income. The claim form requires the submission of various supporting documents such as medical reports, operation reports, and histology reports to substantiate the claim.\",    \"CLAIM_FORM_TYPE\": \"CANCER\"  },  \"CLINICAL_ABSTRACT_APPLICATION\": {    \"NAME_OF_PATIENT\": \"E G L H\",    \"NRIC_FIN_BC\": \"S 152\",    \"NAME\": \"E G.LH\",    \"ADDRESS\": \"BLK A AVE #0 - IS\",    \"NRIC_NO\": \"S 152\",    \"DATE\": \"29 DEC 2021\"  },  \"CLAIMANT_STATEMENT\": {    \"POLICY_DETAILS\": {      \"POLICY_NUMBERS\": \"\",      \"TYPE_OF_CLAIM\": \"Critical Illness\"    },    \"DETAILS_OF_LIFE_ASSURED\": {      \"NAME_OF_LIFE_ASSURED\": \"E G L H\",      \"GENDER\": \"F\",      \"MARITAL_STATUS\": \"MARRIED\"    }  }}"
[[Prototype]]
: 
Object





import React, { useEffect, useState } from "react";
import { useLocation } from "react-router-dom";
import axios from "axios";
import styles from "./Verify.module.css";
import { useNavigate } from "react-router-dom";
import { HashLoader } from "react-spinners";


const Verify = () => {
  const location = useLocation();

  // Define state variables for storing the data, loading state, and error
  const [summary, setSummary] = useState(null);
  const [recommendation, setRecommendation] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);
      const navigate = useNavigate();


const HandleEmail = ()=>{
    
 navigate("/generate-email")
}

  // Use effect to fetch data when component mounts
  useEffect(() => {
    const fetchData = async (recNum) => {
      try {
        // Prepare your payload
        const payload = {
         tasktype: "VERIFY_CLAIM",
         claimid: recNum,
         PSID: "PS391481" 
        };

        // Prepare custom headers (if needed)
        const headers = {
          "Content-Type": "application/json", // Adjust content type based on your needs

        };

        // Example API call with payload and headers
        const response = await axios.post(
          "dummy", // Replace with your actual endpoint
          payload,
          { headers } // Pass headers as part of the request
        );
        
        console.log('Summary:', response.data)

      // Parse the response body (since it's a JSON string)
        const responseBody = JSON.parse(response.data.verifyclaimactdata.body);

        // Extract the summary details and recommendation
        const { CLAIM_FORM_BRIEF_SUMMARY, CLAIM_FORM_TYPE, CLAIM_STATUS, DETAILED_SUMMARY } = responseBody.SUMMARY_DETAILS;

        // Set the state variables with parsed data
        setSummary({
          briefSummary: CLAIM_FORM_BRIEF_SUMMARY,
          claimType: CLAIM_FORM_TYPE,
          claimStatus: CLAIM_STATUS,
        });
        setRecommendation(DETAILED_SUMMARY);
      } catch (error) {
        setError("Failed to load data");
        console.error(error);
      } finally {
        setLoading(false);
      }
    };

    // Check if location.state exists, otherwise fetch the data
    if (!location.state) {
      fetchData();
    } else {
      const { summary, recommendation } = location.state;
      setSummary(summary);
      setRecommendation(recommendation);
      setLoading(false);
    }
  }, [location.state]);

  // Show loading or error message while fetching data
 if (loading) {
    return (
      <div className={styles.spinnerContainer}>
          <HashLoader color="#0f5fdc" size={40} />
      </div>
    );
  }

  if (error) {
    return <p>{error}</p>;
  }

  // Check if summary or recommendation is not available and handle it
  if (!summary || !recommendation) {
    return <p>No data available.</p>;
  }

  return (
    <div className={styles.verifyContainer}>
      {/* Left side: Summary */}
      <div className={styles.leftPanel}>
        <h3>Claim Summary</h3>
        <p><strong>Claim Type:</strong> {summary.claimType}</p>
        <p><strong>Claim Status:</strong> {summary.claimStatus}</p>
      </div>

      {/* Right side: Recommendations */}
      <div className={styles.rightPanel}>
        <h3>Detailed Summary</h3>
        <p>{recommendation}</p>
      </div>
      
      <div className={styles.genrateEmailContainer}>
          <button className={styles.generateEmailButton} onClick={HandleEmail}>
          Generate Email
          </button>
      </div>
    </div>
  );
};

export default Verify;
