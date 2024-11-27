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
  
  
  const handleVerify = () => {
    const claimSummary = data?.CLAIM_FORM_DETAILS?.CLAIM_FORM_DETAILED_SUMMARY;

    // Pass the claim summary to Verify page through state
    navigate("/verify", {
      state: {
        claimSummary, // Pass the detailed summary here
      },
    });
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





import React, { useEffect, useState } from "react";
import { useLocation, useNavigate } from "react-router-dom";
import axios from "axios";
import styles from "./Verify.module.css";
import { HashLoader } from "react-spinners";

const Verify = () => {
  const location = useLocation();
  const navigate = useNavigate();

  // State variables for storing the data, loading state, and error
  const [summary, setSummary] = useState(null);
  const [recommendation, setRecommendation] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  // Handle the navigation to generate email page
  const handleEmail = () => {
    navigate("/generate-email");
  };

  // Fetch the data when the component mounts or when location.state changes
  useEffect(() => {
    const fetchData = async (recNum) => {
      try {
        // Prepare the payload for the API request
        const payload = {
          tasktype: "VERIFY_CLAIM",
          claimid: recNum,
          PSID: "PS391481", // Replace with actual PSID or retrieve it dynamically
        };

        // Custom headers for the API request (if needed)
        const headers = {
          "Content-Type": "application/json", // Adjust content type if needed
        };

        // Send the API request
        const response = await axios.post(
          "dummy", // Replace with actual endpoint URL
          payload,
          { headers }
        );

        // Log the response to verify the data structure
        console.log("API Response:", response.data);

        // Parse the response body (assuming it's a JSON string)
        const responseBody = JSON.parse(response.data.verifyclaimactdata.body);

        // Extract the required fields
        const {
          CLAIM_FORM_BRIEF_SUMMARY,
          CLAIM_FORM_TYPE,
          CLAIM_STATUS,
          CLAIM_FORM_DETAILED_SUMMARY, // Extract the detailed summary here
        } = responseBody.SUMMARY_DETAILS;

        // Set the state with the extracted data
        setSummary({
          briefSummary: CLAIM_FORM_BRIEF_SUMMARY,
          claimType: CLAIM_FORM_TYPE,
          claimStatus: CLAIM_STATUS,
        });
        setRecommendation(CLAIM_FORM_DETAILED_SUMMARY); // Store detailed summary as recommendation
      } catch (error) {
        setError("Failed to load data");
        console.error(error);
      } finally {
        setLoading(false);
      }
    };

    // If location.state exists, use it directly; otherwise, fetch the data
    if (!location.state) {
      fetchData("sampleRecNum"); // Replace with actual recNum or get it from location.state if needed
    } else {
      const { summary, recommendation } = location.state;
      setSummary(summary);
      setRecommendation(recommendation);
      setLoading(false);
    }
  }, [location.state]);

  // Show loading spinner or error message
  if (loading) {
    return (
      <div className={styles.spinnerContainer}>
        <HashLoader color="#0f5fdc" size={40} />
      </div>
    );
  }

  // Show error if the data couldn't be fetched
  if (error) {
    return <p>{error}</p>;
  }

  // Show message if summary or recommendation data is unavailable
  if (!summary || !recommendation) {
    return <p>No data available.</p>;
  }

  return (
    <div className={styles.verifyContainer}>
      {/* Left side: Claim Summary */}
      <div className={styles.leftPanel}>
        <h3>Claim Summary</h3>
        <p>
          <strong>Claim Type:</strong> {summary.claimType}
        </p>
        <p>
          <strong>Claim Status:</strong> {summary.claimStatus}
        </p>
      </div>

      {/* Right side: Detailed Summary (Recommendation) */}
      <div className={styles.rightPanel}>
        <h3>Detailed Summary</h3>
        <p>{recommendation}</p> {/* This will display the CLAIM_FORM_DETAILED_SUMMARY */}
      </div>

      {/* Generate Email Button */}
      <div className={styles.generateEmailContainer}>
        <button className={styles.generateEmailButton} onClick={handleEmail}>
          Generate Email
        </button>
      </div>
    </div>
  );
};

export default Verify;

