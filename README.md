import React, { useState } from 'react';
import { useNavigate } from 'react-router-dom';
import styles from './Personas.module.css';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faArrowRight } from '@fortawesome/free-solid-svg-icons'; // Import FontAwesome icon

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

  

  return (
    <div className={styles.Personas}>
      <h1>Your Personas</h1>
      
      <div className={styles.personaCards}>
        {personas.map((persona, index) => (
          <div
            key={index}
            className={`${styles.personaCard} ${index === currentPersonaIndex ? styles.active : ''}`}
            style={{ backgroundColor: persona.themeColor }}
            
          >
            <h2>{persona.title}</h2>
            <p className={styles.description}>{persona.description}</p>
            <span onClick={() => handleClick(persona.route)}><FontAwesomeIcon icon={faArrowRight} /></span>
          </div>
        ))}
      </div>
    </div>
  );
};

export default Personas;
