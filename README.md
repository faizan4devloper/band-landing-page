import React, { useState } from 'react';
import { useNavigate } from 'react-router-dom';
import styles from './Personas.module.css';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faArrowRight } from '@fortawesome/free-solid-svg-icons';
import { faGraduationCap, faBriefcase, faHeartbeat } from '@fortawesome/free-solid-svg-icons'; // Import icons

const personas = [
  {
    title: 'Education',
    description: 'Streamlined Academic Pathfinding...',
    themeColor: '#e6ebf5',
    route: '/education',
    icon: faGraduationCap, // Icon for education
  },
  {
    title: 'Job',
    description: 'Find job opportunities and career development tools.',
    themeColor: '#e6ebf5',
    route: '/job',
    icon: faBriefcase, // Icon for job
  },
  {
    title: 'Health',
    description: 'Manage health records and access health services.',
    themeColor: '#e6ebf5',
    route: '/health',
    icon: faHeartbeat, // Icon for health
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
      <h1 className={styles.gradientText}>Your Personas</h1>

      <div className={styles.personaCards}>
        {personas.map((persona, index) => (
          <div
            key={index}
            className={`${styles.personaCard} ${index === currentPersonaIndex ? styles.active : ''}`}
            style={{ backgroundColor: persona.themeColor }}
            onClick={() => handleClick(persona.route)}
          >
            <FontAwesomeIcon icon={persona.icon} className={styles.personaIcon} /> {/* Displaying Icon */}
            <h2 className={styles.cardHead}>{persona.title}</h2>
            <p className={styles.description}>{persona.description}</p>
            <div className={styles.rightIcon}>
              <FontAwesomeIcon icon={faArrowRight} />
              <span className={styles.goText}>Go to next page</span>
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

.gradientText {
  background: -webkit-gradient(linear, left top, right top, 
    color-stop(-19.51%, #7abef7), 
    color-stop(36.51%, #4080f5), 
    to(#572ac2));
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  text-fill-color: transparent;
}

.personaCards {
  display: flex;
  gap: 2rem;
  justify-content: center;
  position: relative;
}

.personaCard {
  width: 240px;
  height: 300px; /* Fixed height */
  padding: 1.5rem;
  border-radius: 0.75rem;
  text-align: center;
  background-color: #f2f2f2;
  box-shadow: 0 10px 15px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s ease, background-color 0.3s ease, box-shadow 0.3s ease;
  cursor: pointer;
  overflow: hidden;
  position: relative;
  display: flex;
  flex-direction: column;
  justify-content: space-between; /* Space between icon, title, and description */
}

.personaCard:hover {
  transform: translateY(-8px);
  background: linear-gradient(90deg, rgb(95, 30, 193) 0%, rgb(15, 95, 220) 100%);
  color: white;
  box-shadow: 0 20px 25px rgba(0, 0, 0, 0.15);
}

.personaCard h2 {
  font-size: 1.5rem;
  margin: 0.5rem 0;
  background: -webkit-gradient(linear, left top, right top, 
    color-stop(-19.51%, #7abef7), 
    color-stop(36.51%, #4080f5), 
    to(#572ac2));
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  text-fill-color: transparent;
}

.personaCard:hover h2 {
  color: white;
}

.personaCard .description {
  font-size: 1rem;
  margin: 1rem 0;
  opacity: 0;
  transform: translateY(20px);
  transition: opacity 0.3s ease, transform 0.3s ease;
  color: #555;
}

.personaCard:hover .description {
  opacity: 1;
  transform: translateY(0);
  color: white;
}

.personaIcon {
  font-size: 2.5rem;
  color: #888;
  transition: color 0.3s ease;
}

.personaCard:hover .personaIcon {
  color: white;
}

.rightIcon {
  display: flex;
  align-items: center;
  position: absolute;
  bottom: 1rem;
  right: 1.5rem;
  font-size: 1.5rem;
  color: #888;
  opacity: 1;
  transition: opacity 0.3s ease, transform 0.3s ease;
}

.goText {
  margin-left: 0.5rem;
  opacity: 0;
  transform: translateX(-10px);
  transition: opacity 0.3s ease, transform 0.3s ease;
}

.personaCard:hover .goText {
  opacity: 1;
  transform: translateX(5px);
}

.personaCard:hover .rightIcon {
  transform: translateX(5px);
  color: white;
}
