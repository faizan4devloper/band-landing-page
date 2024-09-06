import React, { useState, useEffect } from "react";
import ReactDOM from "react-dom/client";
import { useNavigate} from "react-router-dom";
import { Document, Page } from "react-pdf";
import Select from "react-select";
// import  NewClaimPage  from "./NewClaimPage"
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
  }
  
  // Mock function to fetch documents
  const fetchDocuments = () => {
    // This could be replaced with an actual API call
    return new Promise((resolve, reject) => {
      // Simulating an API call with setTimeout
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
      }, 1000); // Simulating a 1 second delay
    });
  };

  useEffect(() => {
    fetchDocuments().then((data) => {
      setDocuments(data);
    });
  }, []);

  // const handleDocumentSelect = (event) => {
  //   const selectedUrl = event.target.value;
  //   const selectedDoc = documents.find((doc) => doc.url === selectedUrl);
  //   setSelectedDocUrl(selectedDoc.url);
  //   onDocumentSelect(selectedDoc);
  // };
  const handleDocumentSelect = (selected) => {
    const selectedDoc = documents.find(doc => doc.name === selected.value);
    setSelectedDocUrl(selectedDoc.url);
    onDocumentSelect(selectedDoc);
  }  

  return (
    <div className="hero">
      <div className="hero-content">
        <div className="claim-info">
          <h1>Claim Assist.</h1>
          <p>
           Empower your claim process to minimize Claim Denial and maximize reimbursements.
          </p>
        </div>
        <div className="claim-form">
          <form>
            <h2>Submit Your Claim</h2>
            
            <div className="form-group">
              <label>Type of Claim:</label>
              <div className="radio-btn">
                <label>
                  <input
                    type="radio"
                    value="new"
                    checked={claimType === "new"}
                    // onChange={(e) => setClaimType(e.target.value)}
                    // onClick={()=> navigate('/new-claim')}
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
      options={documents.map(doc => ({value: doc.name, label: doc.name}))}
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
  color: white;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  padding: 20px;
}
 
.hero-content {
  display: flex;
  justify-content: space-between;
  width: 100%;
  max-width: 1200px;
  background-color: rgba(0, 0, 0, 0.5);
  border-radius: 10px;
  padding: 20px;
}
 
.claim-info {
  flex: 1;
  margin: 20px;
}
 
.claim-form {
  flex: 1;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  background-color: rgba(255, 255, 255, 0.8); /* Semi-transparent background */
  color: black;
  border-radius: 10px;
  padding: 20px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2); /* Adding shadow for better depth */
  transition: background-color 0.3s, transform 0.3s;
}
 
.claim-form h2 {
  margin-bottom: 20px;
  font-weight: 600;
  background: -webkit-gradient(
    linear,
    left top,
    right top,
    color-stop(-19.51%, #7abef7),
    color-stop(36.51%, #4080f5),
    to(#572ac2)
  );
  background: -webkit-linear-gradient(
    left,
    #7abef7 -19.51%,
    #4080f5 36.51%,
    #572ac2 100%
  );
  background: -o-linear-gradient(
    left,
    #7abef7 -19.51%,
    #4080f5 36.51%,
    #572ac2 100%
  );
  background: linear-gradient(
    90deg,
    #7abef7 -19.51%,
    #4080f5 36.51%,
    #572ac2 100%
  );
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  text-fill-color: transparent;
  text-transform: inherit !important;
}
 
.form-group {
  margin-bottom: 20px;
  width: 100%;
}
 
.form-group label {
  display: block;
  margin-bottom: 10px;
  text-align: center;
  font-weight: 600;
  color: #3d3d3d; /* Matching with hero section */
}
 
.form-group input[type="radio"] {
  margin-right: 10px;
}
 
.form-group select {
  display: block;
  width: 100%;
  padding: 10px;
  margin-top: 10px;
  border: 1px solid #6f36cd; /* Matching with hero section */
  border-radius: 5px;
}
.radio-btn{
  display: flex;
  justify-content: center; 
}
 
button[type="submit"],
button[type="button"] {
  padding: 10px 20px;
  background-color: #27ae60;
  border: none;
  color: white;
  cursor: pointer;
  transition: background-color 0.3s, transform 0.3s;
  border-radius: 5px;
}
 
button[type="submit"]:hover,
button[type="button"]:hover {
  background-color: #2ecc71;
  transform: scale(1.1);
}
 
button[type="submit"]:focus,
button[type="button"]:focus {
  outline: none;
  box-shadow: 0 0 5px rgba(39, 174, 96, 0.5);
}
