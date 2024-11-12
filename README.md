import React, { useState } from 'react';
import { useNavigate } from 'react-router-dom';
import styles from './Personas.module.css';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faArrowRight, faArrowLeft } from '@fortawesome/free-solid-svg-icons';

import educationImg from '../assets/EducationLogo.png'; 
import jobImg from '../assets/JobLogo.png'; 
import healthImg from '../assets/HealthLogo.png'; 

const personas = [
  {
    title: 'Education',
    description: 'Streamlined Academic Pathfinding...',
    themeColor: '#e6ebf5',
    route: '/education',
    img: educationImg,
  },
  {
    title: 'Job',
    description: 'Find job opportunities and career development tools.',
    themeColor: '#e6ebf5',
    route: '/job',
    img: jobImg,
  },
  {
    title: 'Health',
    description: 'Manage health records and access health services.',
    themeColor: '#e6ebf5',
    route: '/health',
    img: healthImg,
  },
];

const Personas = () => {
  const navigate = useNavigate();

  const handleClick = (route) => {
    navigate(route);
  };

  return (
    <div className={styles.Personas}>
      {/* Back Button */}
      <button className={styles.backButton} onClick={() => navigate(-1)}>
        <FontAwesomeIcon icon={faArrowLeft} className={styles.backIcon} />
        Back
      </button>

      <h1 className={styles.gradientText}>Your Personas</h1>

      <div className={styles.personaCards}>
        {personas.map((persona, index) => (
          <div
            key={index}
            className={styles.personaCard}
            style={{ backgroundColor: persona.themeColor }}
            onClick={() => handleClick(persona.route)}
          >
            <img src={persona.img} alt={persona.title} className={styles.personaImage} />
            <h2 className={styles.cardHead}>{persona.title}</h2>
            <p className={styles.description}>{persona.description}</p>
            <div className={styles.rightIcon}>
              <span className={styles.goText}>Explore</span>
              <FontAwesomeIcon icon={faArrowRight} className={styles.arrow} />
            </div>
          </div>
        ))}
      </div>
    </div>
  );
};

export default Personas;





.backButton {
  display: flex;
  align-items: center;
  background-color: transparent;
  border: none;
  color: #4080f5;
  font-size: 1rem;
  font-weight: 600;
  cursor: pointer;
  margin-bottom: 1.5rem;
  transition: color 0.3s ease, transform 0.3s ease;
}

.backButton:hover {
  color: #572ac2;
  transform: translateX(-5px);
}

.backIcon {
  margin-right: 0.5rem;
  font-size: 1.2rem;
}
