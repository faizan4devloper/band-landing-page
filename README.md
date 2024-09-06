import React, { useState, useEffect } from "react";
import { useNavigate } from "react-router-dom";
import { Document, Page } from "react-pdf";
import Select from "react-select";
import "./Hero.css";

export function Hero({ onDocumentSelect }) {
  const [claimType, setClaimType] = useState("");
  const navigate = useNavigate();
  const [documents, setDocuments] = useState([]);
  const [selectedDocUrl, setSelectedDocUrl] = useState("");

  const handleClaimTypeChange = (e) => {
    setClaimType(e.target.value);
    if (e.target.value === "new") {
      navigate("./NewClaimPage"); 
    }
  };

  const fetchDocuments = () => {
    return new Promise((resolve) => {
      setTimeout(() => {
        const mockDocuments = [
          {
            name: "Document 1",
            url: "https://css4.pub/2017/newsletter/drylab.pdf",
          },
          {
            name: "Document 2",
            url: "https://css4.pub/2017/newsletter/drylab.pdf",
          },
          {
            name: "Document 3",
            url: "https://css4.pub/2017/newsletter/drylab.pdf",
          },
        ];
        resolve(mockDocuments);
      }, 1000);
    });
  };

  useEffect(() => {
    fetchDocuments().then((data) => {
      setDocuments(data);
    });
  }, []);

  const handleDocumentSelect = (selected) => {
    const selectedDoc = documents.find(doc => doc.name === selected.value);
    setSelectedDocUrl(selectedDoc.url);
    onDocumentSelect(selectedDoc);
  };

  return (
    <div className="hero">
      <div className="hero-background-animation"></div>
      <div className="hero-content">
        <div className="claim-info">
          <h1>Claim Assist.</h1>
          <p>
            Empower your claim process to minimize claim denial and maximize reimbursements.
          </p>
        </div>

        <div className="claim-form card">
          <h2>Submit Your Claim</h2>
          <form>
            <div className="form-group">
              <label>Type of Claim:</label>
              <div className="radio-btn">
                <label>
                  <input
                    type="radio"
                    value="new"
                    checked={claimType === "new"}
                    onChange={handleClaimTypeChange}
                  />
                  New
                </label>
                <label>
                  <input
                    type="radio"
                    value="existing"
                    checked={claimType === "existing"}
                    onChange={(e) => setClaimType(e.target.value)}
                  />
                  Existing
                </label>
              </div>
            </div>

            {claimType === "existing" && (
              <div className="form-group">
                <label>Select Existing Claim</label>
                <Select 
                  options={documents.map(doc => ({ value: doc.name, label: doc.name }))}
                  onChange={handleDocumentSelect}
                />
              </div>
            )}

            {selectedDocUrl && (
              <div className="pdf-container">
                <Document file={selectedDocUrl}>
                  <Page pageNumber={1} />
                </Document>
              </div>
            )}
          </form>
        </div>
      </div>
    </div>
  );
}

.hero {
  background: linear-gradient(90deg, #6f36cd 0%, #1f77f6 100%);
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  position: relative;
  color: white;
  padding: 20px;
  overflow: hidden;
}

.hero-background-animation {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: radial-gradient(circle, rgba(255,255,255,0.1), rgba(0,0,0,0.1));
  animation: gradient-animation 15s ease infinite;
  z-index: -1;
}

@keyframes gradient-animation {
  0% { background-position: 0% 50%; }
  50% { background-position: 100% 50%; }
  100% { background-position: 0% 50%; }
}

.hero-content {
  display: flex;
  justify-content: space-between;
  width: 100%;
  max-width: 1200px;
  padding: 20px;
}

.claim-info {
  flex: 1;
  margin-right: 20px;
}

.claim-info h1 {
  font-size: 3rem;
  margin-bottom: 20px;
}

.claim-info p {
  font-size: 1.2rem;
  margin-bottom: 30px;
}

/* Container for the form */
.claim-form {
  background-color: #ffffff;
  padding: 40px;
  border-radius: 12px;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
  max-width: 600px;
  margin: 20px auto;
  font-family: "Roboto", sans-serif;
}

/* Form heading */
.claim-form h2 {
  font-size: 2rem;
  color: #333;
  margin-bottom: 20px;
  text-align: center;
  font-weight: 600;
}

/* Input fields styling */
.form-group {
  margin-bottom: 25px;
  display: flex;
  flex-direction: column;
}

.form-group label {
  font-size: 1.2rem;
  color: #555;
  margin-bottom: 10px;
}

.form-group input[type="text"],
.form-group select {
  padding: 12px;
  font-size: 1rem;
  border: 1px solid #ccc;
  border-radius: 8px;
  transition: border-color 0.3s ease;
  outline: none;
}

/* Focus effect for input fields */
.form-group input[type="text"]:focus,
.form-group select:focus {
  border-color: #1f77f6;
}

/* Radio buttons styling */
.radio-btn {
  display: flex;
  justify-content: center;
  gap: 25px;
}

.radio-btn label {
  font-size: 1rem;
  color: #555;
}

.radio-btn input[type="radio"] {
  margin-right: 10px;
}

/* PDF document container styling */
.pdf-container {
  margin-top: 20px;
  background-color: #f9f9f9;
  padding: 20px;
  border: 2px dashed #ccc;
  border-radius: 8px;
  text-align: center;
}

/* Submit button styling */
.cta-button {
  display: block;
  width: 100%;
  padding: 15px;
  font-size: 1.1rem;
  background-color: #1f77f6;
  color: white;
  border: none;
  border-radius: 10px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.cta-button:hover {
  background-color: #1a60c1;
}

/* Form group styles for a responsive layout */
@media screen and (max-width: 768px) {
  .claim-form {
    padding: 20px;
    max-width: 90%;
  }

  .form-group input[type="text"],
  .form-group select {
    padding: 10px;
  }

  .cta-button {
    padding: 12px;
  }
}
