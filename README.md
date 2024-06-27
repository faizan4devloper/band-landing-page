import React, { useState } from "react";
import { Login } from "./components/Login/Login";
import { Landing } from "./components/Landing/Landing";
import MainContainer from "./components/Education/MainContainer";
import "./App.css";

function App() {
  const [view, setView] = useState("login");
  const [advisorType, setAdvisorType] = useState("");

  const handleLogin = () => {
    setView("landing");
  };

  const handleEnterMain = (type) => {
    setAdvisorType(type);
    setView("main");
  };

  const handleGoBack = () => {
    if (view === "landing") {
      setView("login");
    } else if (view === "main") {
      setView("landing");
    }
  };

  return (
    <div className="app">
      <nav className="breadcrumbs">
        <ul>
          <li>
            <a href="#" onClick={handleGoBack}>
              Login
            </a>
          </li>
          {view === "main" && (
            <>
              <li>
                <a href="#">{advisorType} Advisor</a>
              </li>
            </>
          )}
          {view === "landing" && (
            <li>
              <a>Landing</a>
            </li>
          )}
        </ul>
      </nav>
      {view === "login" && <Login onLogin={handleLogin} />}
      {view === "landing" && <Landing onEnterMain={handleEnterMain} />}
      {view === "main" && <MainContainer advisorType={advisorType}/>}
    </div>
  );
}

export default App;



import React from "react";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faArrowRight } from "@fortawesome/free-solid-svg-icons";
import "./Landing.css";

export function Landing({ onEnterMain }) {
  return (
    <div className="landing">
      <h2>UK Citizen Advisor</h2>
      <div className="cards-container">
        <div className="card">
          <h4>Education Advisor</h4>
          <p>Description about Education Advisor.</p>
          <button onClick={() => onEnterMain('Education')}>
            <FontAwesomeIcon icon={faArrowRight} />
          </button>
        </div>
        <div className="card">
          <h4>Job Advisor</h4>
          <p>Description about Job Advisor.</p>
          <button onClick={() => onEnterMain('Job')}>
            <FontAwesomeIcon icon={faArrowRight} />
          </button>
        </div>
        <div className="card">
          <h4>Health Advisor</h4>
          <p>Description about Health Advisor.</p>
          <button onClick={() => onEnterMain('Health')}>
            <FontAwesomeIcon icon={faArrowRight} />
          </button>
        </div>
      </div>
    </div>
  );
}





