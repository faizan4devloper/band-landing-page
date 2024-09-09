import React, { useState } from "react";
import "./App.css";
import { BrowserRouter, Routes, Route } from "react-router-dom";

import { Header } from "./components/Landing/Header";
import { Hero } from "./components/Landing/Hero";
import { LandingPage } from "./components/Landing/LandingPage"; // Import LandingPage
import { NewClaimPage } from "./NewClaimPage";
import { SummaryView } from "./SummaryView";
import { FilesProvider } from "./FilesContext";
import Breadcrumbs from "./components/Breadcrumbs/Breadcrumbs"; // Import Breadcrumbs

function App() {
  const [selectedDocument, setSelectedDocument] = useState(null);

  const handleDocumentSelect = (document) => {
    setSelectedDocument(document);
  };

  return (
    <BrowserRouter>
      <Header />
      <FilesProvider>
        <Breadcrumbs /> {/* Include Breadcrumbs component */}
        <Routes>
          <Route path="/" element={<LandingPage />} />  {/* LandingPage as the default route */}
          <Route path="/hero" element={<Hero onDocumentSelect={handleDocumentSelect} />} />  {/* Hero page */}
          <Route path="/NewClaimPage" element={<NewClaimPage />} />
          <Route path="/summary" element={<SummaryView />} />
        </Routes>
      </FilesProvider>
    </BrowserRouter>
  );
}

export default App;
