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


import React from 'react';
import { Link, useLocation } from 'react-router-dom';
import './Breadcrumbs.css'; // Custom CSS for styling

const Breadcrumbs = () => {
  const location = useLocation();
  const pathnames = location.pathname.split('/').filter(x => x);

  return (
    <nav className="breadcrumbs">
      <Link to="/">Home</Link>
      {pathnames.map((pathname, index) => {
        const to = `/${pathnames.slice(0, index + 1).join('/')}`;
        return (
          <span key={to}>
            <span className="breadcrumb-separator">/</span>
            <Link to={to}>{pathname.charAt(0).toUpperCase() + pathname.slice(1)}</Link>
          </span>
        );
      })}
    </nav>
  );
};

export default Breadcrumbs;


.breadcrumbs {
  display: flex;
  align-items: center;
  font-size: 1rem;
  color: #333;
  margin: 20px 0;
}

.breadcrumbs a {
  text-decoration: none;
  color: #007bff;
}

.breadcrumbs a:hover {
  text-decoration: underline;
}

.breadcrumb-separator {
  margin: 0 5px;
}
