import React, { useState } from 'react';
import { useNavigate } from 'react-router-dom';
import styles from './Personas.module.css';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faArrowLeft } from '@fortawesome/free-solid-svg-icons'; // Import FontAwesome icon

const personas = [
  {
    title: 'Education',
    description: 'Streamlined Academic Pathfinding: Elevating your experience from disjointed information to directed guidance in placements, appeals, and transfers.',
    themeColor: '#E2D9FB',
    route: '/education',
  },
  {
    title: 'Job',
    description: 'Find job opportunities and career development tools.',
    themeColor: '#D3F9D8',
    route: '/job',
  },
  {
    title: 'Health',
    description: 'Manage health records and access health services.',
    themeColor: '#FCE8E6',
    route: '/health',
  },
];

const Personas = () => {
  const navigate = useNavigate();
  const [currentPersonaIndex, setCurrentPersonaIndex] = useState(0);

  const handleClick = (route) => {
    navigate(route);
  };

  const goToPrevious = () => {
    setCurrentPersonaIndex((prevIndex) => (prevIndex === 0 ? personas.length - 1 : prevIndex - 1));
  };

  return (
    <div className={styles.Personas}>
      <h1>Your Personas</h1>
      <div className={styles.arrowButton} onClick={goToPrevious}>
        <FontAwesomeIcon icon={faArrowLeft} />
      </div>
      <div className={styles.personaCards}>
        {personas.map((persona, index) => (
          <div
            key={index}
            className={`${styles.personaCard} ${index === currentPersonaIndex ? styles.active : ''}`}
            style={{ backgroundColor: persona.themeColor }}
            onClick={() => handleClick(persona.route)}
          >
            <h2>{persona.title}</h2>
            <p className={styles.description}>{persona.description}</p>
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
  position: relative;
}

h1 {
  font-size: 2.5rem;
  color: #333;
  margin-bottom: 2rem;
}

.personaCards {
  display: flex;
  gap: 2rem;
  justify-content: center;
  position: relative;
}

.personaCard {
  width: 200px;
  padding: 2rem;
  border-radius: 0.5rem;
  color: #000;
  text-align: center;
  box-shadow: 0 10px 15px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s ease, background-color 0.3s ease;
  cursor: pointer;
  position: relative;
  overflow: hidden;
}

.personaCard::before {
  content: '';
  position: absolute;
  top: -100%;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.05);
  transition: top 0.3s ease;
}

.personaCard:hover::before {
  top: 0;
}

.personaCard:hover {
  transform: translateY(-10px);
  background-color: #000;
  color: white;
}

.personaCard h2 {
  font-size: 1.5rem;
  margin-bottom: 1rem;
  transition: transform 0.3s ease;
}

.personaCard .description {
  font-size: 1rem;
  transform: translateY(100%);
  opacity: 0;
  transition: transform 0.3s ease, opacity 0.3s ease;
}

.personaCard:hover .description {
  transform: translateY(0);
  opacity: 1;
}

.arrowButton {
  position: absolute;
  top: 10px;
  left: 10px;
  background-color: #fff;
  border-radius: 50%;
  width: 40px;
  height: 40px;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.arrowButton:hover {
  background-color: #e0e0e0;
}

.arrowButton svg {
  font-size: 1.5rem;
  color: #333;
}
