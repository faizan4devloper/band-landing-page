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
            onClick={() => handleClick(persona.route)}
          >
            <h2>{persona.title}</h2>
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
  width: 220px;
  padding: 2rem;
  border-radius: 0.5rem;
  color: #000;
  text-align: center;
  box-shadow: 0 10px 15px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s ease, background-color 0.3s ease, box-shadow 0.3s ease;
  cursor: pointer;
  position: relative;
  overflow: hidden;
}

.personaCard:hover {
  transform: translateY(-8px);
  background-color: #000;
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

.personaCard:hover .description {
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
  transform: translateX(5px); /* Slide in effect for the icon */
}

.rightIcon:hover {
  color: #fff;
  transition: color 0.3s ease;
}
