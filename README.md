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
  const [loading, setLoading] = useState(false);
  const [data, setData] = useState(null);

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

      setData(response.data.allclaimactdata);
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
        <p>
          <strong>Summary:</strong> {parsedData.CLAIM_FORM_DETAILS?.CLAIM_FORM_DETAILED_SUMMARY}
        </p>
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
                onClick={() => handleReload(row.recNum)}
                disabled={loading}
              >
                <FontAwesomeIcon
                  icon={faSync}
                  spin={loading}
                  className={styles.icon}
                />
                {!loading && " Reload"}
              </button>
              <button
                className={styles.verifyButton}
                onClick={() =>
                  handleVerify(
                    row.recNum,
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
            </div>
          ))}
        </div>
        <Modal
          isOpen={isModalOpen}
          onRequestClose={() => setIsModalOpen(false)}
          className={styles.modal}
          overlayClassName={styles.modalOverlay}
          ariaHideApp={false}
        >
          <div className={styles.modalContent}>
            <button
              className={styles.closeButton}
              onClick={() => setIsModalOpen(false)}
            >
              Close
            </button>
            <iframe
              src=""
              title="Preview"
              className={styles.modalIframe}
            ></iframe>
          </div>
        </Modal>
      </div>
    </div>
  );
};

export default MainContent;




import React, { useEffect, useState } from "react";
import { useLocation } from "react-router-dom";
import axios from "axios";
import styles from "./Verify.module.css";
import { useNavigate } from "react-router-dom";
import { HashLoader } from "react-spinners";

const Verify = () => {
  const location = useLocation();
  const navigate = useNavigate();

  const recNum = location.state?.recNum || "CL1234567";
  const summary = location.state?.summary || "No Summary Available";

  const HandleEmail = () => {
    navigate("/generate-email", {
      state: { summary, recNum },
    });
  };

  const [loading, setLoading] = useState(false);
  const [error, setError] = useState(null);

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

  return (
    <div>
      <div className={styles.claimIdDisplay}>
        <h3>Claim ID: {recNum}</h3>
      </div>
      <div className={styles.verifyContainer}>
        <div className={styles.leftPanel}>
          <h3>Claim Summary</h3>
          <p><strong>Summary:</strong> {summary}</p>
        </div>
        <div className={styles.genrateEmailContainer}>
          <button className={styles.generateEmailButton} onClick={HandleEmail}>
            Generate Email
          </button>
        </div>
      </div>
    </div>
  );
};

export default Verify;
