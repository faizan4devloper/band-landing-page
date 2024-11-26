import React, { useState } from "react";
import axios from "axios";
import Modal from "react-modal";
import styles from "./MainContent.module.css";

const MainContent = ({ message, rows, setRows }) => {
  const [isModalOpen, setIsModalOpen] = useState(false);
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

      console.log(response.data);

      setData(response.data.allclaimactdata); // Store fetched data
    } catch (error) {
      console.error("Failed to fetch data for RecNum:", recNum, error);
    } finally {
      setLoading(false);
    }
  };

  const openModal = (previewLink) => {
    setModalContent(previewLink);
    setIsModalOpen(true);
  };

  const closeModal = () => {
    setIsModalOpen(false);
    setModalContent(null);
  };

  return (
    <div className={styles.mainContent}>
      <div className={styles.previewSection}>
        <h3>Document Preview</h3>
        {data?.document_name?.length > 0 ? (
          <ul className={styles.documentList}>
            {data.document_name.map((doc, index) => (
              <li key={index}>
                <button
                  className={styles.previewButton}
                  onClick={() => openModal(doc.S)}
                >
                  {doc.S}
                </button>
              </li>
            ))}
          </ul>
        ) : (
          <p>No document available</p>
        )}
      </div>

      <div className={styles.extractContentSection}>
        <h3>Extract Content</h3>
        {loading ? (
          <p>Loading...</p>
        ) : data?.total_extracted_data ? (
          <pre className={styles.codeBlock}>
            <code>{data.total_extracted_data}</code>
          </pre>
        ) : (
          <p>No data available</p>
        )}

        <button
          className={styles.reloadButton}
          onClick={() => handleReload("CL928697")}
          disabled={loading}
        >
          Reload
        </button>
      </div>

      <Modal
        isOpen={isModalOpen}
        onRequestClose={closeModal}
        className={styles.modal}
        overlayClassName={styles.modalOverlay}
        ariaHideApp={false}
      >
        <div className={styles.modalContent}>
          <button className={styles.closeButton} onClick={closeModal}>
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


.mainContent {
  display: flex;
  gap: 20px;
  padding: 20px;
}

.previewSection,
.extractContentSection {
  flex: 1;
  padding: 10px;
  background: #fff;
  border: 1px solid #ddd;
  border-radius: 5px;
}

.documentList {
  list-style: none;
  padding: 0;
}

.documentList li {
  margin-bottom: 10px;
}

.previewButton {
  background-color: #007bff;
  color: white;
  padding: 8px 12px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.previewButton:hover {
  background-color: #0056b3;
}

.codeBlock {
  background: #2d2d2d;
  color: #f8f8f2;
  padding: 15px;
  border-radius: 5px;
  overflow-x: auto;
  font-family: "Courier New", Courier, monospace;
  font-size: 14px;
  line-height: 1.5;
  border: 1px solid #444;
}

.codeBlock code {
  display: block;
  white-space: pre-wrap;
  word-wrap: break-word;
}

.reloadButton {
  margin-top: 10px;
  background-color: #28a745;
  color: white;
  padding: 8px 12px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.reloadButton:disabled {
  background-color: #ccc;
  cursor: not-allowed;
}
