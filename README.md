import React, { useState, useEffect } from "react";
import axios from "axios";
import { useNavigate } from "react-router-dom";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faChevronRight } from "@fortawesome/free-solid-svg-icons";
import styles from "./MainContent.module.css";
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

  useEffect(() => {
    if (rows.length > 0) {
      handleReload(rows[0].recNum);
    }
  }, [rows]);

  useEffect(() => {
    if (data && data.total_extracted_data) {
      handleDocumentEmbedQueue();
    }
  }, [data]);

  const handleDocumentEmbedQueue = async () => {
    if (rows.length === 0) return;

    const currentClaimId = rows[0].recNum;
    const claimFormPath = data?.total_extracted_data 
      ? JSON.parse(data?.total_extracted_data).CLAIM_FORM_DETAILS?.CLAIM_FORM_PATH 
      : null;

    if (!claimFormPath) {
      console.warn("No claim form path found");
      return;
    }

    try {
      const payload = { claimid: currentClaimId, recnumber: currentClaimId, claimformpath: claimFormPath };
      const response = await axios.post("https:docembedqueue", payload, { headers: { "Content-Type": "application/json" } });
      setEmbeddingStatus(response.data);
    } catch (error) {
      console.error("Failed to call document embed queue:", error);
      setEmbeddingStatus(null);
    }
  };

  useEffect(() => {
    const fetchPercentage = async () => {
      if (rows.length === 0 || !rows[0]?.recNum) return;
      const currentClaimId = rows[0].recNum;
      setIsLoading(true);
      try {
        const response = await axios.post("https://percentage", { claimid: currentClaimId }, { headers: { "Content-Type": "application/json" } });
        let percentageValue = response.data?.body?.empty_key_perc ? parseFloat(response.data.body.empty_key_perc.replace("%", "")) : 0;
        setPercentage(percentageValue);
      } catch (error) {
        setPercentage(0);
      } finally {
        setIsLoading(false);
      }
    };
    fetchPercentage();
  }, [rows, data]);

  const handleVerify = () => {
    const formType = data?.total_extracted_data?.CLAIM_FORM_DETAILS?.CLAIM_FORM_TYPE || "No Claim Form Type";
    const summary = data?.total_extracted_data?.CLAIM_FORM_DETAILS?.CLAIM_FORM_DETAILED_SUMMARY || "No Summary Available";
    navigate("/verify", { state: { clid, summary, selectedPolicy, percentage } });
  };

  const handleReload = async (recNum) => {
    setLoading(true);
    try {
      const response = await axios.post("https:all", { tasktype: "FETCH_SINGLE_ACT_CLAIM", claimid: recNum }, { headers: { "Content-Type": "application/json" } });
      setData(response.data.allclaimactdata);
    } catch (error) {
      console.error("Failed to fetch data for RecNum:", recNum, error);
    } finally {
      setLoading(false);
    }
  };

  return (
    <div className={styles.mainContentWrapper}>
      {rows.length > 0 && <h3 className={styles.claimId}>Claim ID: {rows[0].recNum}</h3>}
      <div className={styles.mainGrid}>
        <div className={styles.leftColumn}>
          <ClaimClassification data={data?.total_extracted_data ? JSON.parse(data.total_extracted_data) : null} uploadedFileName={uploadedFileName} />
          <DocumentPreview staticPreviewUrl={staticPreviewUrl} />
        </div>
        <div className={styles.rightColumn}>
          <ClaimProcessingStatus percentage={percentage} isLoading={isLoading} />
          <ExtractedContent data={data} loading={loading} rows={rows} handleReload={handleReload} />
          <button className={styles.verifyButton} onClick={handleVerify} disabled={!rows.length}>
            Verify Claim <FontAwesomeIcon icon={faChevronRight} />
          </button>
        </div>
      </div>
    </div>
  );
};

export default MainContent;
