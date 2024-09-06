import React from "react";
import { useNavigate } from "react-router-dom";  // Import useNavigate
import "./LandingPage.css";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faFileAlt } from "@fortawesome/free-solid-svg-icons";

export const LandingPage = () => {
  const navigate = useNavigate();  // Initialize navigate function

  const handleButtonClick = () => {
    navigate("/hero");  // Navigate to Hero page
  };

  return (
    <div className="landing-page">
      <div className="welcome-container">
        <h1 className="welcome-text animate-fade-in">Welcome to Claim Assist</h1>
        <p className="welcome-subtext animate-slide-in">
          Get started with your claim process easily and efficiently.
        </p>
        <button onClick={handleButtonClick} className="btn animate-bounce">
          <FontAwesomeIcon icon={faFileAlt} className="btn-icon" /> Start Your Claim
        </button>
      </div>
    </div>
  );
};




import React, { useState } from "react";
import "./App.css";
import { BrowserRouter, Routes, Route } from "react-router-dom";

import { Header } from "./components/Landing/Header";
import { Hero } from "./components/Landing/Hero";
import { LandingPage } from "./components/Landing/LandingPage";
import { NewClaimPage } from "./NewClaimPage";
import { SummaryView } from "./SummaryView";
import { FilesProvider } from "./FilesContext";

function App() {
  const [selectedDocument, setSelectedDocument] = useState(null);

  const handleDocumentSelect = (document) => {
    setSelectedDocument(document);
  };

  return (
    <BrowserRouter>
      <Header />
      <FilesProvider>
        <Routes>
          <Route path="/" element={<LandingPage />} />  {/* Landing page */}
          <Route path="/hero" element={<Hero onDocumentSelect={handleDocumentSelect} />} />  {/* Hero page */}
          <Route path="/NewClaimPage" element={<NewClaimPage />} />
          <Route path="/summary" element={<SummaryView />} />
        </Routes>
      </FilesProvider>
    </BrowserRouter>
  );
}

export default App;
