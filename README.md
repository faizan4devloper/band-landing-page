import React from "react";
import "./LandingPage.css";  // Custom CSS for the page
import { Link } from "react-router-dom";
import { FaFileAlt } from "react-icons/fa";  // Importing an icon from FontAwesome

export const LandingPage = () => {
  return (
    <div className="landing-page">
      <div className="welcome-container">
        <h1 className="welcome-text animate-fade-in">Welcome to Claim Assist</h1>
        <p className="welcome-subtext animate-slide-in">
          Get started with your claim process easily and efficiently.
        </p>
        <Link to="/NewClaimPage" className="btn animate-bounce">
          <FaFileAlt className="btn-icon" /> Start Your Claim
        </Link>
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
  animation: fade-in 1.5s ease-in-out;
}

.welcome-subtext {
  font-size: 1.2rem;
  margin-top: 10px;
  animation: slide-in 1.8s ease-in-out;
}

.btn {
  display: inline-flex;
  align-items: center;
  background-color: #6a11cb;
  color: white;
  padding: 12px 24px;
  border: none;
  font-size: 1.2rem;
  cursor: pointer;
  border-radius: 8px;
  transition: transform 0.3s ease;
  text-decoration: none;
}

.btn:hover {
  transform: scale(1.05);
}

.btn-icon {
  margin-right: 10px;
  font-size: 1.5rem;
}

.animate-bounce {
  animation: bounce 1.5s infinite;
}

@keyframes bounce {
  0%, 100% {
    transform: translateY(0);
  }
  50% {
    transform: translateY(-5px);
  }
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
