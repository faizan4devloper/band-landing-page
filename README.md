
import React, { useState } from 'react';
import { useNavigate } from 'react-router-dom';
import styles from './Personas.module.css';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faArrowRight } from '@fortawesome/free-solid-svg-icons';

const personas = [
  {
    title: 'Education',
    description: 'Streamlined Academic Pathfinding...',
    themeColor: '#e6ebf5',
    route: '/education',
  },
  {
    title: 'Job',
    description: 'Find job opportunities and career development tools.',
    themeColor: '#e6ebf5',
    route: '/job',
  },
  {
    title: 'Health',
    description: 'Manage health records and access health services.',
    themeColor: '#e6ebf5',
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



.Personas {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
  background-color: rgba(255, 255, 255, 1); /* Keeping white background */
}

.gradientText {
  background: -webkit-gradient(linear, left top, right top, 
    color-stop(-19.51%, #7abef7), 
    color-stop(36.51%, #4080f5), 
    to(#572ac2));
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  text-fill-color: transparent; /* For compatibility with non-Webkit browsers */
}

.personaCards {
  display: flex;
  gap: 2rem;
  justify-content: center;
  position: relative;
}

.personaCard {
  width: 240px;
  padding: 2rem;
  border-radius: 0.75rem;
  text-align: center;
  background-color: #f2f2f2; /* Updated based on persona theme */
  box-shadow: 0 10px 15px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s ease, background-color 0.3s ease, box-shadow 0.3s ease;
  cursor: pointer;
  overflow: hidden;
  position: relative;
}

.personaCard:hover {
  transform: translateY(-8px);
  background: linear-gradient(90deg, rgb(95, 30, 193) 0%, rgb(15, 95, 220) 100%);
  color: white;
  box-shadow: 0 20px 25px rgba(0, 0, 0, 0.15);
}

.personaCard h2 {
  font-size: 1.8rem;
  margin-bottom: 1rem;
  background: -webkit-gradient(linear, left top, right top, 
    color-stop(-19.51%, #7abef7), 
    color-stop(36.51%, #4080f5), 
    to(#572ac2));
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  text-fill-color: transparent; /* For compatibility with non-Webkit browsers */
}

/* Change gradient text to white on hover */
.personaCard:hover h2 {
  color: white; /* Change text color to white */
}

.personaCard .description {
  font-size: 1rem;
  margin-bottom: 1rem;
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

/* Style for the right icon and text */
.rightIcon {
  display: flex;
  align-items: center;
  position: absolute;
  bottom: .8rem;
  left: -1rem; /* Set to left to start on the left side */
  font-size: .8rem;
  transition: color 0.3s ease;
  
  white-space: nowrap;
}

/* Arrow will move on hover */
.rightIcon .arrow {
  margin-left: 0.3rem; /* Space between text and arrow */
  transition: transform 0.3s ease; /* Smooth transition */
}

.goText {
  opacity: 0; /* Initially hidden */
  font-size: 1rem;
  transform: translateX(-110px); /* Initially moved to the left */
  transition: opacity 0.3s ease, transform 0.3s ease; /* Smooth transition */
  color: white; /* Change text color */
}

/* Text appears on hover with sliding effect */
.personaCard:hover .goText {
  opacity: 1; /* Makes text visible */
  transform: translateX(70px); /* Moves text to its original position */
}

/* Move the arrow to the right on hover */
.personaCard:hover .arrow {
  transform: translateX(80px); /* Adjust to create a sliding effect */
}


.personaCard:hover .gradientText{
  background:#fff;
}
