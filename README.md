import React, { useState } from 'react';
import { BrowserRouter as Router, Route, Routes } from 'react-router-dom';
import Personas from './components/Personas/Personas';
import EducationPage from './components/Pages/Education/EducationPage';
import JobPage from './components/Pages/Job/JobPage';
import HealthPage from './components/Pages/Health/HealthPage';
import LoginPage from './components/Login/LoginPage';
import Header from './components/Header/Header';

function App() {
  
  const [isAuthenticated, setIsAuthenticated]= useState(false)
  
  return (
    <Router>
      <div className="App">
        <Header />
        <Routes>
          <Route path="/" element={<LoginPage setIsAuthenticated={setIsAuthenticated}/>} />
          <Route path="/personas" element={<Personas />} />
          <Route path="/education" element={<EducationPage />} />
          <Route path="/job" element={<JobPage />} />
          <Route path="/health" element={<HealthPage />} />
        </Routes>
      </div>
    </Router>
  );
}

export default App;





