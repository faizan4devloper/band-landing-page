import React from "react";
import "./LandingPage.css";  // Create a separate CSS file for animations and styling
import { Link } from "react-router-dom";

export const LandingPage = () => {
  return (
    <div className="landing-page">
      <div className="welcome-container">
        <h1 className="welcome-text animate-fade-in">Welcome to Claim Assist</h1>
        <p className="welcome-subtext animate-slide-in">
          Your journey to a seamless claim experience starts here.
        </p>
        <div className="cta-buttons">
          <Link to="/NewClaimPage" className="btn animate-pop">Start a New Claim</Link>
          <Link to="/summary" className="btn-secondary animate-pop">View Summary</Link>
        </div>
      </div>
    </div>
  );
};




.landing-page {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100vh;
  background: linear-gradient(to right, #74ebd5, #ACB6E5);
}

.welcome-container {
  text-align: center;
  color: white;
}

.welcome-text {
  font-size: 2.5rem;
  animation: fade-in 2s ease-out;
}

.welcome-subtext {
  font-size: 1.2rem;
  margin-top: 10px;
  animation: slide-in 2.5s ease-out;
}

.cta-buttons {
  margin-top: 20px;
}

.btn, .btn-secondary {
  padding: 10px 20px;
  border: none;
  font-size: 1rem;
  cursor: pointer;
  transition: transform 0.3s ease;
}

.btn {
  background-color: #6a11cb;
  color: white;
}

.btn-secondary {
  background-color: #2575fc;
  color: white;
}

.btn:hover, .btn-secondary:hover {
  transform: scale(1.05);
}

/* Animations */
@keyframes fade-in {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}

@keyframes slide-in {
  from {
    transform: translateY(50px);
    opacity: 0;
  }
  to {
    transform: translateY(0);
    opacity: 1;
  }
}

@keyframes pop {
  0%, 100% {
    transform: scale(1);
  }
  50% {
    transform: scale(1.1);
  }
}

.animate-fade-in {
  animation: fade-in 1.5s ease-in-out;
}

.animate-slide-in {
  animation: slide-in 1.8s ease-in-out;
}

.animate-pop {
  animation: pop 0.5s ease-in-out;
}




import { LandingPage } from "./components/Landing/LandingPage";  // Add the path to your LandingPage

function App() {
  return (
    <BrowserRouter>
      <Header />
      <FilesProvider>
        <Routes>
          <Route path="/" element={<LandingPage />} />  {/* Set LandingPage as the default route */}
          <Route path="/NewClaimPage" element={<NewClaimPage />} />
          <Route path="/summary" element={<SummaryView />} />
        </Routes>
      </FilesProvider>
    </BrowserRouter>
  );
}

export default App;
