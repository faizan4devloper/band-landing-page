import React, { useState } from 'react';
import { useNavigate } from 'react-router-dom';
import styles from './Personas.module.css';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faArrowRight } from '@fortawesome/free-solid-svg-icons';

const personas = [
  {
    title: 'Education',
    description: 'Streamlined Academic Pathfinding: Elevating your experience from disjointed information to directed guidance in placements, appeals, and transfers.',
    themeColor: '#d0e2ff',
    route: '/education',
  },
  {
    title: 'Job',
    description: 'Find job opportunities and career development tools.',
    themeColor: '#cce1ff',
    route: '/job',
  },
  {
    title: 'Health',
    description: 'Manage health records and access health services.',
    themeColor: '#e6e0ff',
    route: '/health',
  },
];

const Personas = () => {
  const navigate = useNavigate();
  const [stacked, setStacked] = useState(true); // State to manage card stacking

  const handleClick = (route) => {
    setStacked(false); // Remove stacking on click
    navigate(route);
  };

  return (
    <div className={styles.Personas}>
      <h1>Your Personas</h1>

      <div className={`${styles.personaCards} ${!stacked ? styles.inline : ''}`}>
        {personas.map((persona, index) => (
          <div
            key={index}
            className={`${styles.personaCard} ${stacked ? styles.stacked : ''}`}
            style={{ backgroundColor: persona.themeColor }}
            onClick={() => handleClick(persona.route)}
          >
            <h2 className={styles.cardHead}>{persona.title}</h2>
            <p className={styles.description}>{persona.description}</p>
            <div className={styles.rightIcon}>
              <FontAwesomeIcon icon={faArrowRight} />
            </div>
          </div>
        ))}
      </div>
    </div>
  );
};

export default Personas;



.Personas {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
  background-color: rgba(255, 255, 255, 1);
}

h1 {
  font-size: 2.5rem;
  color: rgb(95, 30, 193);
  margin-bottom: 2rem;
}

.personaCards {
  display: flex;
  gap: 2rem;
  justify-content: center;
  position: relative;
  transition: all 0.5s ease; /* Smooth transition for inline layout */
}

.personaCards.inline {
  gap: 2rem; /* Normal spacing between cards */
}

.personaCard {
  width: 220px;
  padding: 2rem;
  border-radius: 0.5rem;
  color: #000;
  text-align: center;
  box-shadow: 0 10px 15px rgba(0, 0, 0, 0.1);
  transition: transform 0.5s ease, background-color 0.5s ease, box-shadow 0.5s ease;
  cursor: pointer;
  position: relative;
  overflow: hidden;
  transform-origin: center center;
  opacity: 1;
}

.personaCard.stacked {
  position: absolute;
  z-index: -1; /* Cards behind each other */
  transform: rotate(-5deg) translateY(-50px); /* Stacked with slight rotation */
  opacity: 0.6; /* Lower opacity for the "behind" look */
  transition: transform 0.5s ease, opacity 0.5s ease; /* Smooth transition to normal */
}

.personaCards.inline .personaCard {
  z-index: 1;
  transform: rotate(0) translateY(0); /* Bring them in-line */
  opacity: 1; /* Fully visible */
}

.personaCard:hover {
  transform: translateY(-8px);
  background-color: rgb(95, 30, 193);
  color: white;
  box-shadow: 0 20px 25px rgba(0, 0, 0, 0.15);
}

.personaCard h2 {
  font-size: 1.6rem;
  margin-bottom: 1rem;
}

.personaCard .description {
  font-size: 1rem;
  margin-bottom: 1rem;
  opacity: 0;
  transform: translateY(20px);
  transition: opacity 0.3s ease, transform 0.3s ease;
}

.personaCard:hover .description, .cardHead {
  opacity: 1;
  transform: translateY(0);
}

.rightIcon {
  display: flex;
  justify-content: flex-end;
  align-items: center;
  position: absolute;
  bottom: 1rem;
  right: 1.5rem;
  font-size: 1.5rem;
  color: #333;
  opacity: 0;
  transition: opacity 0.3s ease, transform 0.3s ease;
}

.personaCard:hover .rightIcon {
  opacity: 1;
  transform: translateX(5px);
}
