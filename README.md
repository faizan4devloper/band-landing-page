import React, { useState } from "react";
import { useNavigate } from "react-router-dom";
import { HashLoader } from "react-spinners";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faEnvelope, faAnglesRight, faChevronDown, faChevronUp } from "@fortawesome/free-solid-svg-icons";
import styles from "./Verify.module.css";
import Chatbot from "./Chatbot";
import ClaimProcessingStatus from "../MainContent/ClaimProcessingStatus";
import VerificationDB from "./VerificationDB";
import Insights from "./Insights";

const VerifyContent = ({
  recNum,
  psid,
  Dsummary,
  summary,
  recommendation,
  loading,
  error,
  emptyKeysPercentage,
}) => {
  const navigate = useNavigate();

  // Collapsible states
  const [isVerificationSummaryOpen, setVerificationSummaryOpen] = useState(true);
  const [isVerificationDBOpen, setVerificationDBOpen] = useState(true);

  const handleEmail = () => {
    navigate("/generate-email", {
      state: { Dsummary, summary, recommendation, recNum, psid },
    });
  };

  if (loading) {
    return (
      <div className={styles.spinnerContainer}>
        <HashLoader color="#0f5fdc" size={40} />
      </div>
    );
  }

  if (error) {
    return <div className={styles.errorContainer}><p>Error: {error}</p></div>;
  }

  return (
    <div className={styles.verifyMainContainer}>
      {/* Claim ID and Status */}
      <div className={styles.headerSection}>
        <div className={styles.claimIdDisplay}>
          <div className={styles.claimIdBadge}>
            <span>Claim ID</span>
            <h3>{recNum || "N/A"}</h3>
          </div>
          <div className={styles.claimIdBadge}>
            <span>PS ID</span>
            <h3>{psid || "N/A"}</h3>
          </div>
        </div>
        <div className={styles.statusSection}>
          <ClaimProcessingStatus percentage={emptyKeysPercentage} isLoading={loading} />
        </div>
      </div>

      <div className={styles.verifyContainer}>
        {/* Chatbot */}
        <div>
          <Chatbot />
        </div>

        {/* Left and Right Panels */}
        <div className={styles.mainPanels}>
          {/* Left Panel - (Unchanged) */}
          <div className={styles.leftPanel}>
            <div className={styles.panelHeader}>
              <h3>Claim Summary</h3>
            </div>
            <div className={styles.panelContent}>
              <p>{Dsummary || "No summary available"}</p>
            </div>

            {/* Insights Section */}
            <div className={styles.insightsPanel}>
              <div className={styles.panelHeader}>
                <h3>Insights From Historic Claims</h3>
              </div>
              <div className={styles.panelContent}>
                <Insights claimType={summary?.claimType || "GENERAL"} />
              </div>
            </div>
          </div>

          {/* Right Panel - Verification Summary & VerificationDB in a Column Layout */}
          <div className={styles.rightPanel}>
            {/* Verification Summary (Collapsible) */}
            <div className={styles.verificationPanel}>
              <div className={styles.panelHeader} onClick={() => setVerificationSummaryOpen(!isVerificationSummaryOpen)}>
                <h3>Verification Summary</h3>
                <FontAwesomeIcon icon={isVerificationSummaryOpen ? faChevronUp : faChevronDown} className={styles.toggleIcon} />
              </div>
              <div className={`${styles.collapsibleContent} ${isVerificationSummaryOpen ? styles.open : styles.closed}`}>
                <div className={styles.summarySection}>
                  <h4>Detailed Recommendation</h4>
                  <p>{recommendation || "No detailed recommendation available"}</p>
                </div>
              </div>
            </div>

            {/* Verification With Integrated System (Collapsible) */}
            <div className={styles.verificationPanel}>
              <div className={styles.panelHeader} onClick={() => setVerificationDBOpen(!isVerificationDBOpen)}>
                <h3>Verification With Integrated System</h3>
                <FontAwesomeIcon icon={isVerificationDBOpen ? faChevronUp : faChevronDown} className={styles.toggleIcon} />
              </div>
              <div className={`${styles.collapsibleContent} ${isVerificationDBOpen ? styles.open : styles.closed}`}>
                <VerificationDB recNum={recNum} psid={psid} />
              </div>
            </div>
          </div>
        </div>
      </div>

      {/* Generate Email Button */}
      <div className={styles.generateEmailContainer}>
        <button className={styles.generateEmailButton} onClick={handleEmail} disabled={!summary || !recommendation}>
          <FontAwesomeIcon icon={faEnvelope} className={styles.emailIcon} />
          Generate Email <FontAwesomeIcon icon={faAnglesRight} />
        </button>
      </div>
    </div>
  );
};

export default VerifyContent;




.mainPanels {
  display: flex;
  gap: 15px;
}

/* Left Panel (Unchanged) */
.leftPanel {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 15px;
}

/* Right Panel - Column Layout */
.rightPanel {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 15px;
}

/* Collapsible Panels */
.verificationPanel {
  background-color: white;
  border-radius: 12px;
  border: 1px solid #e1e4e8;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.05);
  overflow: hidden;
}

.collapsibleContent {
  max-height: 0;
  overflow: hidden;
  transition: max-height 0.4s ease-in-out, opacity 0.3s ease-in-out;
  opacity: 0;
}

.collapsibleContent.open {
  max-height: 500px;
  opacity: 1;
}

.collapsibleContent.closed {
  max-height: 0;
  opacity: 0;
}

/* Panel Header */
.panelHeader {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background-color: #f1f5f9;
  padding: 15px 20px;
  cursor: pointer;
  transition: background 0.3s ease;
}

.panelHeader:hover {
  background-color: #e1e4e8;
}

.toggleIcon {
  font-size: 1.2rem;
  color: #0f5fdc;
  transition: transform 0.3s ease;
}

/* Insights Panel */
.insightsPanel {
  background-color: white;
  border-radius: 12px;
  border: 1px solid #e1e4e8;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.05);
  padding: 20px;
}

/* Generate Email Button */
.generateEmailContainer {
  display: flex;
  justify-content: center;
  margin-top: 20px;
}








import React, { useState } from "react";
import { useNavigate } from "react-router-dom";
import { HashLoader } from "react-spinners";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faEnvelope, faAnglesRight, faChevronDown, faChevronUp } from "@fortawesome/free-solid-svg-icons";
import styles from "./Verify.module.css";
import Chatbot from "./Chatbot";
import ClaimProcessingStatus from "../MainContent/ClaimProcessingStatus";
import VerificationDB from "./VerificationDB";
import Insights from "./Insights";

const VerifyContent = ({
  recNum,
  psid,
  Dsummary,
  summary,
  recommendation,
  loading,
  error,
  emptyKeysPercentage,
}) => {
  const navigate = useNavigate();

  // State for collapsible sections
  const [isVerificationSummaryOpen, setVerificationSummaryOpen] = useState(true);
  const [isVerificationDBOpen, setVerificationDBOpen] = useState(true);

  const handleEmail = () => {
    navigate("/generate-email", {
      state: { Dsummary, summary, recommendation, recNum, psid },
    });
  };

  if (loading) {
    return (
      <div className={styles.spinnerContainer}>
        <HashLoader color="#0f5fdc" size={40} />
      </div>
    );
  }

  if (error) {
    return <div className={styles.errorContainer}><p>Error: {error}</p></div>;
  }

  return (
    <div className={styles.verifyMainContainer}>
      {/* Claim ID and Status */}
      <div className={styles.headerSection}>
        <div className={styles.claimIdDisplay}>
          <div className={styles.claimIdBadge}>
            <span>Claim ID</span>
            <h3>{recNum || "N/A"}</h3>
          </div>
          <div className={styles.claimIdBadge}>
            <span>PS ID</span>
            <h3>{psid || "N/A"}</h3>
          </div>
        </div>
        <div className={styles.statusSection}>
          <ClaimProcessingStatus percentage={emptyKeysPercentage} isLoading={loading} />
        </div>
      </div>

      <div className={styles.verifyContainer}>
        {/* Chatbot */}
        <div>
          <Chatbot />
        </div>

        {/* Left and Right Panels */}
        <div className={styles.mainPanels}>
          {/* Left Panel - Claim Summary & Insights */}
          <div className={styles.leftPanel}>
            <div className={styles.panelHeader}>
              <h3>Claim Summary</h3>
            </div>
            <div className={styles.panelContent}>
              <p>{Dsummary || "No summary available"}</p>
            </div>

            {/* Insights Section */}
            <div className={styles.insightsPanel}>
              <div className={styles.panelHeader}>
                <h3>Insights From Historic Claims</h3>
              </div>
              <div className={styles.panelContent}>
                <Insights claimType={summary?.claimType || "GENERAL"} />
              </div>
            </div>
          </div>

          {/* Right Panel - Verification Summary & VerificationDB (Column Layout) */}
          <div className={styles.rightPanel}>
            {/* Parent Verification Section */}
            <div className={styles.verificationSection}>
              {/* Verification Summary (Collapsible) */}
              <div className={styles.verificationPanel}>
                <div className={styles.panelHeader} onClick={() => setVerificationSummaryOpen(!isVerificationSummaryOpen)}>
                  <h3>Verification Summary</h3>
                  <FontAwesomeIcon icon={isVerificationSummaryOpen ? faChevronUp : faChevronDown} className={styles.toggleIcon} />
                </div>
                <div className={`${styles.collapsibleContent} ${isVerificationSummaryOpen ? styles.open : styles.closed}`}>
                  <div className={styles.summarySection}>
                    <h4>Detailed Recommendation</h4>
                    <p>{recommendation || "No detailed recommendation available"}</p>
                  </div>
                </div>
              </div>

              {/* Verification With Integrated System (Collapsible) */}
              <div className={styles.verificationPanel}>
                <div className={styles.panelHeader} onClick={() => setVerificationDBOpen(!isVerificationDBOpen)}>
                  <h3>Verification With Integrated System</h3>
                  <FontAwesomeIcon icon={isVerificationDBOpen ? faChevronUp : faChevronDown} className={styles.toggleIcon} />
                </div>
                <div className={`${styles.collapsibleContent} ${isVerificationDBOpen ? styles.open : styles.closed}`}>
                  <VerificationDB recNum={recNum} psid={psid} />
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>

      {/* Generate Email Button */}
      <div className={styles.generateEmailContainer}>
        <button className={styles.generateEmailButton} onClick={handleEmail} disabled={!summary || !recommendation}>
          <FontAwesomeIcon icon={faEnvelope} className={styles.emailIcon} />
          Generate Email <FontAwesomeIcon icon={faAnglesRight} />
        </button>
      </div>
    </div>
  );
};

export default VerifyContent;




.mainPanels {
  display: flex;
  gap: 15px;
}

.leftPanel {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 15px;
}

/* Right Panel - Column Layout */
.rightPanel {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 15px;
}

/* Verification Section */
.verificationSection {
  display: flex;
  flex-direction: column;
  gap: 15px;
}

/* Collapsible Panels */
.verificationPanel {
  background-color: white;
  border-radius: 12px;
  border: 1px solid #e1e4e8;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.05);
  overflow: hidden;
}

.collapsibleContent {
  max-height: 0;
  overflow: hidden;
  transition: max-height 0.4s ease-in-out, opacity 0.3s ease-in-out;
  opacity: 0;
}

.collapsibleContent.open {
  max-height: 500px;
  opacity: 1;
}

.collapsibleContent.closed {
  max-height: 0;
  opacity: 0;
}

/* Panel Header */
.panelHeader {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background-color: #f1f5f9;
  padding: 15px 20px;
  cursor: pointer;
  transition: background 0.3s ease;
}

.panelHeader:hover {
  background-color: #e1e4e8;
}

.toggleIcon {
  font-size: 1.2rem;
  color: #0f5fdc;
  transition: transform 0.3s ease;
}

/* Insights Panel */
.insightsPanel {
  background-color: white;
  border-radius: 12px;
  border: 1px solid #e1e4e8;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.05);
  padding: 20px;
}

/* Generate Email Button */
.generateEmailContainer {
  display: flex;
  justify-content: center;
  margin-top: 20px;
}




import React, { useState } from "react";
import { useNavigate } from "react-router-dom";
import { HashLoader } from "react-spinners";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faEnvelope, faAnglesRight, faChevronDown, faChevronUp } from "@fortawesome/free-solid-svg-icons";
import styles from "./Verify.module.css";
import Chatbot from "./Chatbot";
import ClaimProcessingStatus from "../MainContent/ClaimProcessingStatus";
import VerificationDB from "./VerificationDB";
import Insights from "./Insights";

const VerifyContent = ({
  recNum,
  psid,
  Dsummary,
  summary,
  recommendation,
  loading,
  error,
  emptyKeysPercentage,
}) => {
  const navigate = useNavigate();

  // State for collapsible sections
  const [isVerificationSummaryOpen, setVerificationSummaryOpen] = useState(true);
  const [isVerificationDBOpen, setVerificationDBOpen] = useState(true);

  const handleEmail = () => {
    navigate("/generate-email", {
      state: { Dsummary, summary, recommendation, recNum, psid },
    });
  };

  if (loading) {
    return (
      <div className={styles.spinnerContainer}>
        <HashLoader color="#0f5fdc" size={40} />
      </div>
    );
  }

  if (error) {
    return <div className={styles.errorContainer}><p>Error: {error}</p></div>;
  }

  return (
    <div className={styles.verifyMainContainer}>
      {/* Claim ID and Status */}
      <div className={styles.headerSection}>
        <div className={styles.claimIdDisplay}>
          <div className={styles.claimIdBadge}>
            <span>Claim ID</span>
            <h3>{recNum || "N/A"}</h3>
          </div>
          <div className={styles.claimIdBadge}>
            <span>PS ID</span>
            <h3>{psid || "N/A"}</h3>
          </div>
        </div>
        <div className={styles.statusSection}>
          <ClaimProcessingStatus percentage={emptyKeysPercentage} isLoading={loading} />
        </div>
      </div>

      <div className={styles.verifyContainer}>
        {/* Chatbot */}
        <div>
          <Chatbot />
        </div>

        {/* Left and Right Panels */}
        <div className={styles.rightLeft}>
          {/* Claim Summary */}
          <div className={styles.leftPanel}>
            <div className={styles.panelHeader}>
              <h3>Claim Summary</h3>
            </div>
            <div className={styles.panelContent}>
              <p>{Dsummary || "No summary available"}</p>
            </div>
          </div>

          {/* Verification Summary (Collapsible) */}
          <div className={styles.rightPanel}>
            <div className={styles.panelHeader} onClick={() => setVerificationSummaryOpen(!isVerificationSummaryOpen)}>
              <h3>Verification Summary</h3>
              <FontAwesomeIcon icon={isVerificationSummaryOpen ? faChevronUp : faChevronDown} className={styles.toggleIcon} />
            </div>
            <div className={`${styles.collapsibleContent} ${isVerificationSummaryOpen ? styles.open : styles.closed}`}>
              <div className={styles.summarySection}>
                <h4>Detailed Recommendation</h4>
                <p>{recommendation || "No detailed recommendation available"}</p>
              </div>
            </div>
          </div>
        </div>

        {/* Insights (Always Visible) */}
        <div className={styles.insightsPanel}>
          <div className={styles.panelHeader}>
            <h3>Insights From Historic Claims</h3>
          </div>
          <div className={styles.panelContent}>
            <Insights claimType={summary?.claimType || "GENERAL"} />
          </div>
        </div>

        {/* VerificationDB (Collapsible) */}
        <div className={styles.verificationDBPanel}>
          <div className={styles.panelHeader} onClick={() => setVerificationDBOpen(!isVerificationDBOpen)}>
            <h3>Verification With Integrated System</h3>
            <FontAwesomeIcon icon={isVerificationDBOpen ? faChevronUp : faChevronDown} className={styles.toggleIcon} />
          </div>
          <div className={`${styles.collapsibleContent} ${isVerificationDBOpen ? styles.open : styles.closed}`}>
            <VerificationDB recNum={recNum} psid={psid} />
          </div>
        </div>
      </div>

      {/* Generate Email Button */}
      <div className={styles.generateEmailContainer}>
        <button className={styles.generateEmailButton} onClick={handleEmail} disabled={!summary || !recommendation}>
          <FontAwesomeIcon icon={faEnvelope} className={styles.emailIcon} />
          Generate Email <FontAwesomeIcon icon={faAnglesRight} />
        </button>
      </div>
    </div>
  );
};

export default VerifyContent;





import React, { useState } from "react";
import { useNavigate } from "react-router-dom";
import { HashLoader } from "react-spinners";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faEnvelope, faAnglesRight, faChevronDown, faChevronUp } from "@fortawesome/free-solid-svg-icons";
import styles from "./Verify.module.css";
import Chatbot from "./Chatbot";
import ClaimProcessingStatus from "../MainContent/ClaimProcessingStatus";
import VerificationDB from "./VerificationDB";
import Insights from "./Insights";

const VerifyContent = ({
  recNum,
  psid,
  Dsummary,
  summary,
  recommendation,
  loading,
  error,
  emptyKeysPercentage,
}) => {
  const navigate = useNavigate();

  // State to control visibility of sections
  const [isVerificationSummaryOpen, setVerificationSummaryOpen] = useState(true);
  const [isVerificationDBOpen, setVerificationDBOpen] = useState(true);

  const handleEmail = () => {
    navigate("/generate-email", {
      state: { Dsummary, summary, recommendation, recNum, psid },
    });
  };

  if (loading) {
    return (
      <div className={styles.spinnerContainer}>
        <HashLoader color="#0f5fdc" size={40} />
      </div>
    );
  }

  if (error) {
    return <div className={styles.errorContainer}><p>Error: {error}</p></div>;
  }

  return (
    <div className={styles.verifyMainContainer}>
      {/* Claim ID and Status */}
      <div className={styles.headerSection}>
        <div className={styles.claimIdDisplay}>
          <div className={styles.claimIdBadge}>
            <span>Claim ID</span>
            <h3>{recNum || "N/A"}</h3>
          </div>
          <div className={styles.claimIdBadge}>
            <span>PS ID</span>
            <h3>{psid || "N/A"}</h3>
          </div>
        </div>
        <div className={styles.statusSection}>
          <ClaimProcessingStatus percentage={emptyKeysPercentage} isLoading={loading} />
        </div>
      </div>

      <div className={styles.verifyContainer}>
        {/* Chatbot */}
        <div>
          <Chatbot />
        </div>

        {/* Left and Right Panels */}
        <div className={styles.rightLeft}>
          {/* Claim Summary */}
          <div className={styles.leftPanel}>
            <div className={styles.panelHeader}>
              <h3>Claim Summary</h3>
            </div>
            <div className={styles.panelContent}>
              <p>{Dsummary || "No summary available"}</p>
            </div>
          </div>

          {/* Verification Summary (Collapsible) */}
          <div className={styles.rightPanel}>
            <div className={styles.panelHeader} onClick={() => setVerificationSummaryOpen(!isVerificationSummaryOpen)}>
              <h3>Verification Summary</h3>
              <FontAwesomeIcon icon={isVerificationSummaryOpen ? faChevronUp : faChevronDown} className={styles.toggleIcon} />
            </div>
            <div className={`${styles.collapsibleContent} ${isVerificationSummaryOpen ? styles.open : styles.closed}`}>
              <div className={styles.summarySection}>
                <h4>Detailed Recommendation</h4>
                <p>{recommendation || "No detailed recommendation available"}</p>
              </div>
            </div>
          </div>
        </div>

        {/* VerificationDB (Collapsible) */}
        <div className={styles.verificationDBPanel}>
          <div className={styles.panelHeader} onClick={() => setVerificationDBOpen(!isVerificationDBOpen)}>
            <h3>Verification With Integrated System</h3>
            <FontAwesomeIcon icon={isVerificationDBOpen ? faChevronUp : faChevronDown} className={styles.toggleIcon} />
          </div>
          <div className={`${styles.collapsibleContent} ${isVerificationDBOpen ? styles.open : styles.closed}`}>
            <VerificationDB recNum={recNum} psid={psid} />
          </div>
        </div>
      </div>

      {/* Generate Email Button */}
      <div className={styles.generateEmailContainer}>
        <button className={styles.generateEmailButton} onClick={handleEmail} disabled={!summary || !recommendation}>
          <FontAwesomeIcon icon={faEnvelope} className={styles.emailIcon} />
          Generate Email <FontAwesomeIcon icon={faAnglesRight} />
        </button>
      </div>
    </div>
  );
};

export default VerifyContent;





/* Smooth transition for collapsible content */
.collapsibleContent {
  max-height: 0;
  overflow: hidden;
  transition: max-height 0.4s ease-in-out, opacity 0.3s ease-in-out;
  opacity: 0;
}

.collapsibleContent.open {
  max-height: 500px; /* Adjust height based on content */
  opacity: 1;
}

.collapsibleContent.closed {
  max-height: 0;
  opacity: 0;
}

/* Toggle Icon */
.panelHeader {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background-color: #f1f5f9;
  padding: 15px 20px;
  cursor: pointer;
  transition: background 0.3s ease;
}

.panelHeader:hover {
  background-color: #e1e4e8;
}

.toggleIcon {
  font-size: 1.2rem;
  color: #0f5fdc;
  transition: transform 0.3s ease;
}

.generateEmailContainer {
  display: flex;
  justify-content: center;
  margin-top: 20px;
}

.generateEmailButton {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 12px 24px;
  background: linear-gradient(90deg, #0f5fdc, #7ca2e1, #0f5fdc);
  color: white;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.3s ease;
}

.generateEmailButton:hover {
  background-color: #1e40af;
  transform: translateY(-2px);
}

.generateEmailButton:disabled {
  background-color: #a0aec0;
  cursor: not-allowed;
}








i want the Verification Summary and Verification With Integrated System in one parent component top Verification Summary bottom Verification With Integrated System and the Verification Summary and Verification With Integrated System section will expand and collapse in beutiful way icon and enhacned the style.



import React from "react";
import { useNavigate } from "react-router-dom";
import { HashLoader } from "react-spinners";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faEnvelope, faAnglesRight } from "@fortawesome/free-solid-svg-icons";
import styles from "./Verify.module.css";
import Chatbot from "./Chatbot";
import ClaimProcessingStatus from "../MainContent/ClaimProcessingStatus";
import VerificationDB from "./VerificationDB";
import Insights from "./Insights"; // Import the Insights component

const VerifyContent = ({
  recNum,
  psid,
  Dsummary,
  summary,
  recommendation,
  loading,
  error,
  emptyKeysPercentage,
}) => {
  const navigate = useNavigate();

  const HandleEmail = () => {
    navigate("/generate-email", {
      state: {
        Dsummary,
        summary,
        recommendation,
        recNum,
        psid,
      },
    });
  };

  // Render loading state
  if (loading) {
    return (
      <div className={styles.spinnerContainer}>
        <HashLoader color="#0f5fdc" size={40} />
      </div>
    );
  }

  // Render error state
  if (error) {
    return (
      <div className={styles.errorContainer}>
        <p>Error: {error}</p>
      </div>
    );
  }

  // Render no data state
  if (!summary || !recommendation) {
    return (
      <div className={styles.noDataContainer}>
        <p>No verification data available.</p>
      </div>
    );
  }

  return (
  <div className={styles.verifyMainContainer}>
    {/* Claim ID and Status Section */}
    <div className={styles.headerSection}>
      <div className={styles.claimIdDisplay}>
        <div className={styles.claimIdBadge}>
          <span>Claim ID</span>
          <h3>{recNum || 'N/A'}</h3>
        </div>
        <div className={styles.claimIdBadge}>
          <span>PS ID</span>
          <h3>{psid || 'N/A'}</h3>
        </div>
      </div>
      <div className={styles.statusSection}>
        <ClaimProcessingStatus percentage={emptyKeysPercentage} isLoading={loading} />
      </div>
    </div>

    {/* Main Content Layout */}
    <div className={styles.verifyContainer}>
      <div>
        <Chatbot />
      </div>

      {/* Left and Right Panels */}
      <div className={styles.rightLeft}>
        {/* Claim Summary */}
        <div className={styles.leftPanel}>
          <div className={styles.panelHeader}>
            <h3>Claim Summary</h3>
          </div>
          <div className={styles.panelContent}>
            <p>{Dsummary || 'No summary available'}</p>
          </div>
        </div>

        {/* Verification Summary */}
        <div className={styles.rightPanel}>
          <div className={styles.panelHeader}>
            <h3>Verification Summary</h3>
          </div>
          <div className={styles.panelContent}>
            <div className={styles.summarySection}>
              <h4>Detailed Recommendation</h4>
              <p>{recommendation || 'No detailed recommendation available'}</p>
            </div>
            <div className={styles.claimDetails}>
  
  
</div>
          </div>
        </div>
      </div>

      {/* Insights & VerificationDB (Inline Layout) */}
      <div className={styles.bottomPanel}>
        <div className={styles.insightsPanel}>
          <div className={styles.panelHeader}>
            <h3>Insights From Historic Claims
</h3>
          </div>
          <Insights claimType={summary?.claimType || 'GENERAL'} />
        </div>
        <div className={styles.verificationDBPanel}>
          <div className={styles.panelHeader}>
            <h3>Verification With Integrated System</h3>
          </div>
          <VerificationDB recNum={recNum} psid={psid} />
        </div>
      </div>
    </div>

    {/* Generate Email Button */}
    <div className={styles.generateEmailContainer}>
      <button 
        className={styles.generateEmailButton} 
        onClick={HandleEmail}
        disabled={!summary || !recommendation}
      >
        <FontAwesomeIcon icon={faEnvelope} className={styles.emailIcon} />
          Generate Email <FontAwesomeIcon icon={faAnglesRight} />
      </button>
    </div>
  </div>
);
};

export default VerifyContent;

.verifyMainContainer {
  max-width: 1400px;
  margin: 0 auto;
  padding: 20px;
  background-color: #f4f6f9;
  border-radius: 12px;
}

/* Header Section */
.headerSection {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
  gap: 20px;
}

.claimIdDisplay {
  display: flex;
  gap: 20px;
  flex-grow: 1;
}

.claimIdBadge {
  flex: 1;
  background-color: white;
  border-radius: 10px;
  padding: 15px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.05);
  transition: transform 0.3s ease;
}

.claimIdBadge:hover {
  transform: translateY(-5px);
}

.claimIdBadge span {
  display: block;
  color: #6b7280;
  font-size: 0.9rem;
  margin-bottom: 8px;
  text-transform: uppercase;
  letter-spacing: 1px;
}

.claimIdBadge h3 {
  margin: 0;
  color: #2c3e50;
  font-size: 1.2rem;
  font-weight: 600;
}

.rightLeft {
  display: flex;
  gap: 10px;
}

.leftPanel, .rightPanel, .insightsPanel, .verificationDBPanel {
  flex: 1;
  background-color: white;
  border-radius: 12px;
  border: 1px solid #e1e4e8;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.05);
  overflow: hidden;
}
.panelHeader {
  background-color: #f1f5f9;
  padding: 15px 20px;
}

.panelHeader h3 {
  margin: 0;
  color: #2c3e50;
  font-size: 1.1rem;
}

.panelContent {
  padding: 20px;
}

.summarySection h4 {
  color: #0f5fdc;
  margin-bottom: 10px;
  padding-bottom: 5px;
}

.claimDetails {
  margin-top: 20px;
  background-color: #f9fafb;
  border-radius: 8px;
  padding: 15px;
}

.claimDetailItem {
  display: flex;
  justify-content: space-between;
  margin-bottom: 10px;
  padding-bottom: 10px;
  border-bottom: 1px solid #e2e8f0;
}

.claimDetailItem:last-child {
  border-bottom: none;
}

.claimDetailItem strong {
  color: #2c3e50;
}

/*.claimDetailItem span {*/
/*  color: #4a5568;*/
/*}*/

/* Bottom Panel (Insights & VerificationDB) */
.bottomPanel {
  display: flex;
  gap: 10px;
  margin-top: 20px;
}

.insightsPanel, .verificationDBPanel {
  flex: 1;
  background-color: white;
  border-radius: 12px;
  border: 1px solid #e1e4e8;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.05);
  overflow: hidden;
  /*padding: 20px;*/
}



.generateEmailContainer {
  display: flex;
  justify-content: center;
  margin-top: 20px;
}

.generateEmailButton {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 12px 24px;
  background:linear-gradient(90deg, #0f5fdc, #7ca2e1, #0f5fdc);
  color: white;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.3s ease;
}

.generateEmailButton:hover {
  background-color: #1e40af;
  transform: translateY(-2px);
}

.generateEmailButton:disabled {
  background-color: #a0aec0;
  cursor: not-allowed;
}

.emailIcon {
  font-size: 1.2rem;
}


/* Spinner */
.spinnerContainer {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

/*.claimProcessingStatusContainer {*/
/*  margin-bottom: 20px;*/
/*  width: 100%;*/
/*  display: flex;*/
/*  justify-content: center;*/
/*}*/
.claimProcessingStatusContainer {
  background-color: #ffffff;
  border-radius: 16px;
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.08);
  padding: 20px;
  display: flex;
  flex-direction: column;
  gap: 15px;
  transition: all 0.3s ease;
  max-width: 350px;
  /*margin: 0 auto;*/
}
.statusSection{
          /* flex: 1 1; */
    background-color: white;
    border-radius: 10px;
    padding: 10px;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.05);
    transition: transform 0.3s ease;
}

.statusSection:hover {
  transform: translateY(-5px);
}

.badge {
  padding: 4px 8px;
  border-radius: 4px;
  font-weight: bold;
  text-transform: uppercase;
  font-size: 0.8em;
}

.badgeSuccess {
     background-color: #e6f7ff;
    color: #28a745;
    border-radius: 20px;
    background-color: #d4edda;
    color: #28a745;
    /* border: 1px solid #52c41a; */
}

.badgeDanger {
  background-color: #fff1f0;
  color: #f5222d;
  border: 1px solid #f5222d;
}

.badgeWarning {
  background-color: #fffbe6;
  color: #faad14;
  border: 1px solid #faad14;
}

