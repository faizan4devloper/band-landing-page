import React, { useState } from "react";
import axios from "axios";
import Modal from "react-modal";
import styles from "./MainContent.module.css";
import { useNavigate } from "react-router-dom";

import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faSync } from '@fortawesome/free-solid-svg-icons';


const MainContent = ({ message, rows, setRows, staticPreviewUrl }) => {
  console.log("Rows:",rows)
  const [isModalOpen, setIsModalOpen] = useState(false);
    const navigate = useNavigate();

  const [modalContent, setModalContent] = useState(null);
  const [loading, setLoading] = useState(false);
  const [data, setData] = useState(null); // To store fetched data

// const handleVerify = (recNum) =>{
//     // navigate("/verify");
//     navigate('/verify', {
//       state: { recNum }
//     } );
//   }

const handleVerify = (recNum, summary) => {
    navigate('/verify', {
      state: { recNum, summary }, // Pass recNum and summary to Verify
    });
  };


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
         <p><strong>Summary:</strong> {parsedData.CLAIM_FORM_DETAILS?.CLAIM_FORM_DETAILED_SUMMARY}</p>

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
  <div>
    
    {rows.length > 0 && (
          <div className={styles.claimIdDisplay}>
          <h3>Claim ID: {rows[0].recNum}</h3>  
          </div>
        )}
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
    <button
                className={styles.verifyButton}
                onClick={() =>
                  handleVerify(
                    rows.recNum,
                    data?.total_extracted_data
                      ? JSON.parse(data?.total_extracted_data).CLAIM_FORM_DETAILS
                          ?.CLAIM_FORM_DETAILED_SUMMARY
                      : "No Summary Available"
                  )
                }
                disabled={!rows.length}
              >
                Verify
              </button>

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
    </div>

);
};

export default MainContent;




/* Main container layout */
.mainContent {
  display: grid;
  grid-template-columns: 1fr 1fr; /* Two equal-width columns */
  grid-template-rows: auto 60px; /* Rows: auto for content, fixed height for the button */
  gap: 20px; /* Space between grid items */
  padding: 20px; /* Space around the entire container */
  align-items: start; /* Aligns sections to the top */
}

/* Left Section: Document Preview */
.previewSection {
  background: #ffffff; /* White background for better contrast */
  padding: 15px;
  border: 1px solid #ddd; /* Subtle border */
  border-radius: 8px;
}

.documentList {
  list-style: none; /* Remove default list styling */
  padding: 0;
  margin: 0;
}

.documentList li {
  margin-bottom: 10px;
}

.previewButton {
  background-color: #007bff; /* Blue background */
  color: white;
  padding: 10px 15px; /* Inner spacing */
  border: none;
  border-radius: 5px;
  cursor: pointer;
  font-size: 14px; /* Adjusted font size for buttons */
  transition: background-color 0.3s ease; /* Smooth hover transition */
}

.previewButton:hover {
  background-color: #0056b3; /* Darker blue on hover */
}

/* Right Section: Extracted Content */
.extractContentSection {
  background: #ffffff; /* White background for better contrast */
  padding: 15px;
  border: 1px solid #ddd; /* Subtle border */
  border-radius: 8px;
}

/* "Verify" Button Container */
.verifyButtonContainer {
  grid-column: span 2; /* Span across both columns in the grid */
  display: flex;
  justify-content: center; /* Horizontally center the button */
  align-items: center; /* Vertically center the button */
  margin-top: 10px; /* Space above the button */
}

/* "Verify" Button Styling */
.verifyButton {
  padding: 10px 180px; /* Spacing around text */
  font-size: 16px; /* Larger font size for visibility */
  font-weight: bold; /* Make text bold */
  background: linear-gradient(90deg, #0f5fdc, #7ca2e1, #0f5fdc);
  color: white; /* White text */
  border: none;
  border-radius: 8px; /* Rounded corners */
  cursor: pointer;
  transition: background-color 0.3s ease, transform 0.2s ease; /* Smooth transition for hover effects */
}



.verifyButton:active {
  background-color: #1e7e34; /* Even darker green when clicked */
  transform: scale(0.95); /* Shrink effect on click */
}

/* Modal Styling */
.modal {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background: #ffffff;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3);
  width: 80%;
  max-width: 800px;
}

.modalOverlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
  z-index: 10;
}

.modalContent {
  position: relative;
}

.closeButton {
  position: absolute;
  top: 10px;
  right: 10px;
  background: #ff4d4d;
  color: white;
  border: none;
  border-radius: 50%;
  width: 30px;
  height: 30px;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  font-size: 16px;
}

.closeButton:hover {
  background: #cc0000;
}

/* Additional General Styles */
.readableContent {
  background: #f8f8f8;
  padding: 15px;
  border-radius: 8px;
  line-height: 1.6;
}

.readableContent h4 {
  color: #333;
  margin-bottom: 10px;
  text-decoration: underline;
}

.readableContent p {
  margin: 5px 0;
  color: #555;
}

.readableContent strong {
  color: #000;
}

h5 {
  margin-top: 15px;
  font-size: 1.1rem;
  color: #444;
}

.reloadButton {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px; /* Space between icon and text */
  margin-top: 10px;
  background: linear-gradient(90deg, #0f5fdc, #7ca2e1, #0f5fdc);
  color: white;
  padding: 10px 20px;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  font-size: 14px;
  font-weight: bold;
  transition: background-color 0.3s ease, transform 0.2s ease;
}



.reloadButton:disabled {
  background-color: #ccc; /* Gray color when disabled */
  cursor: not-allowed;
}

.icon {
  font-size: 18px; /* Icon size */
}

.documentPreviewIframe{
  width: 100%;
  height: 670px;
  border: none;
}

.claimIdDisplay{
     margin: 0px 25px;
    font-size: 16px;
    margin-bottom: 0px;
    font-weight: bold;
    color: #333;
}
