import React, { useState } from "react";
import axios from "axios";
import Modal from "react-modal";
import styles from "./MainContent.module.css";
import { useNavigate } from "react-router-dom";

import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faSync } from '@fortawesome/free-solid-svg-icons';


const MainContent = ({ message, rows, clid, setRows, staticPreviewUrl }) => {
  console.log("Rows:",rows)
  console.log("clid:",clid)
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
const rcNumber = "";
const handleVerify = (clid, summary) => {
  console.log("In Handle Verify RecNum:",clid);
    navigate('/verify', {
      state: { clid, summary }, // Pass recNum and summary to Verify
    });
  };
  



  const handleReload = async (recNum) => {
    setLoading(true);
    try {
      const payload = {
        tasktype: "FETCH_SINGLE_ACT_CLAIM",
        claimid: recNum,
      };

      const response = await axios.post(`https://e21wxu9skj.execute-api.us-east-1.amazonaws.com/dev/querequest`, payload, {
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
                  handleVerify(clid,data?.total_extracted_data
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

