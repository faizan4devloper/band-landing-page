import React, { useState } from "react";
import axios from "axios";
import Modal from "react-modal";
import styles from "./MainContent.module.css";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faSync } from "@fortawesome/free-solid-svg-icons";

const MainContent = ({ message, rows, setRows }) => {
  const [isModalOpen, setIsModalOpen] = useState(false);
  const [modalContent, setModalContent] = useState(null);
  const [loading, setLoading] = useState(false);
  const [data, setData] = useState(null); // To store fetched data
  const [uploadedFile, setUploadedFile] = useState(null); // To store local preview URL

  const handleReload = async (recNum) => {
    setLoading(true);
    try {
      const payload = {
        tasktype: "FETCH_SINGLE_ACT_CLAIM",
        claimid: recNum,
      };

      const response = await axios.post(DUMMY, payload, {
        headers: { "Content-Type": "application/json" },
      });

      console.log("Api", response.data);

      setData(response.data.allclaimactdata); // Store fetched data
    } catch (error) {
      console.error("Failed to fetch data for RecNum:", recNum, error);
    } finally {
      setLoading(false);
    }
  };

  const handleUpload = (event) => {
    const file = event.target.files[0];
    if (file) {
      const fileURL = URL.createObjectURL(file); // Create a local object URL for preview
      setUploadedFile(fileURL);
      console.log("Uploaded file preview ready:", fileURL);
    }
  };

  const renderReadableContent = (data) => {
    if (!data) return <p>No data available</p>;

    const parsedData = JSON.parse(data);

    return (
      <div className={styles.readableContent}>
        <h4>Claim Form Details</h4>
        <p>
          <strong>Type:</strong>{" "}
          {parsedData.CLAIM_FORM_DETAILS?.CLAIM_FORM_TYPE}
        </p>

        <h4>Clinical Abstract Application</h4>
        <p>
          <strong>Name of Patient:</strong>{" "}
          {parsedData.CLINICAL_ABSTRACT_APPLICATION?.NAME_OF_PATIENT}
        </p>
        <p>
          <strong>NRIC/FIN/BC:</strong>{" "}
          {parsedData.CLINICAL_ABSTRACT_APPLICATION?.NRIC_FIN_BC}
        </p>
        <p>
          <strong>Address:</strong>{" "}
          {parsedData.CLINICAL_ABSTRACT_APPLICATION?.ADDRESS}
        </p>
        <p>
          <strong>Date:</strong>{" "}
          {parsedData.CLINICAL_ABSTRACT_APPLICATION?.DATE}
        </p>

        <h4>Claimant Statement</h4>
        <h5>Policy Details</h5>
        <p>
          <strong>Policy Numbers:</strong>{" "}
          {parsedData.CLAIMANT_STATEMENT?.POLICY_DETAILS?.POLICY_NUMBERS}
        </p>
        <p>
          <strong>Type of Claim:</strong>{" "}
          {parsedData.CLAIMANT_STATEMENT?.POLICY_DETAILS?.TYPE_OF_CLAIM}
        </p>

        <h5>Details of Life Assured</h5>
        <p>
          <strong>Name:</strong>{" "}
          {parsedData.CLAIMANT_STATEMENT?.DETAILS_OF_LIFE_ASSURED?.NAME_OF_LIFE_ASSURED}
        </p>
        <p>
          <strong>Gender:</strong>{" "}
          {parsedData.CLAIMANT_STATEMENT?.DETAILS_OF_LIFE_ASSURED?.GENDER}
        </p>
        <p>
          <strong>Marital Status:</strong>{" "}
          {parsedData.CLAIMANT_STATEMENT?.DETAILS_OF_LIFE_ASSURED?.MARITAL_STATUS}
        </p>
      </div>
    );
  };

  return (
    <div className={styles.mainContent}>
      {/* Left: Document Preview */}
      <div className={styles.previewSection}>
        <h3>Document Preview</h3>
        <input
          type="file"
          accept="application/pdf"
          onChange={handleUpload}
          className={styles.uploadInput}
        />
        {uploadedFile ? (
          <iframe
            src={uploadedFile}
            title="PDF Preview"
            className={styles.previewIframe}
          ></iframe>
        ) : (
          <p>No document uploaded yet</p>
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

        <button
          className={styles.reloadButton}
          onClick={() => handleReload("CL928697")}
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

      {/* Centered Verify Button */}
      <div className={styles.verifyButtonContainer}>
        <button
          className={styles.verifyButton}
          onClick={() => console.log("Verify action triggered")}
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
