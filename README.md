import React, { useState } from 'react';
import { useNavigate } from 'react-router-dom';
import styles from './Personas.module.css';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faArrowRight } from '@fortawesome/free-solid-svg-icons';

// Import your images
import educationImg from './assets/education.png'; // Update path as necessary
import jobImg from './assets/job.png'; // Update path as necessary
import healthImg from './assets/health.png'; // Update path as necessary

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
      <h1 className={styles.gradientText}>Your Personas</h1>

      <div className={styles.personaCards}>
        {personas.map((persona, index) => (
          <div
            key={index}
            className={styles.personaCard}
            style={{ backgroundColor: persona.themeColor }}
            onClick={() => handleClick(persona.route)}
          >
            <img src={persona.img} alt={persona.title} className={styles.personaImage} /> {/* Displaying Image */}
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


.personaCard {
  /* Existing styles */
  position: relative; /* Required to position the image absolutely */
  overflow: hidden; /* Ensure the image stays within the card */
}

.personaImage {
  width: 100%; /* Adjust size as needed */
  height: auto; /* Maintain aspect ratio */
  border-radius: 0.75rem; /* Match card's border radius */
  object-fit: cover; /* Ensure image covers the area */
  position: absolute; /* Allow for overlapping with other elements */
  top: 0; /* Position it at the top */
  left: 0; /* Align to the left */
  right: 0; /* Stretch to the right */
  bottom: 50%; /* Cover half the height for a nice effect */
  transition: transform 0.3s ease; /* Smooth transition effect */
}

.personaCard:hover .personaImage {
  transform: scale(1.1); /* Scale up on hover for an effect */
}

.personaCard h2,
.personaCard p,
.rightIcon {
  position: relative; /* Ensure these are on top of the image */
  z-index: 1; /* Layering so that text is above the image */
}
