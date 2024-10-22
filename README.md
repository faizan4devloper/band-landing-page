import React from 'react';
import { useNavigate } from 'react-router-dom';
import styles from './Personas.module.css';

const personas = [
  {
    title: 'Education',
    description: 'Streamlined Academic Pathfinding: Elavating your exprience from disjointed information to directed guidence an placements, appeals, and transfers',
    themeColor: '#E2D9FB', // Example color for education
    route: '/education',
  },
  {
    title: 'Job',
    description: 'Find job opportunities and career development tools.',
    themeColor: '#fcfcfc', // Example color for jobs
    route: '/job',
  },
  {
    title: 'Health',
    description: 'Manage health records and access health services.',
    themeColor: '#fcfcfc', // Example color for health
    route: '/health',
  },
];

const Personas = () => {
  const navigate = useNavigate();

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
            className={styles.personaCard}
            style={{ backgroundColor: persona.themeColor }}
            onClick={() => handleClick(persona.route)}
          >
            <h2>{persona.title}</h2>
            <p>{persona.description}</p>
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
  /*background-color:#f7f7fc;*/
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
}

.personaCard {
  width: 200px;
  padding: 2rem;
  border-radius: 0.5rem;
  color: #000;
  text-align: center;
-webkit-box-shadow: 0 10px 15px rgba(0, 0, 0, .03);
    box-shadow: 0 10px 15px rgba(0, 0, 0, .03);  transition: transform 0.3s ease;
    cursor:pointer;
}

.personaCard:hover {
  transform: translateY(-10px);
  background: #000;
}

.personaCard h2 {
  font-size: 1.5rem;
  margin-bottom: 1rem;
}

.personaCard p {
  font-size: 1rem;
}


    /*border-radius: 5px;*/
    /*height: 100%;*/
    /*-webkit-box-shadow: 0 10px 15px rgba(0, 0, 0, .03);*/
    /*box-shadow: 0 10px 15px rgba(0, 0, 0, .03);*/
    /*border: none;*/
    /*overflow: hidden;*/
    /*background: #fcfcfc;*/
    /*padding-bottom: 1.875rem;*/
