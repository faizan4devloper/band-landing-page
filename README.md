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



import React from 'react';

const EducationPage = () => {
  return (
    <div>
      <h1>Education</h1>
      <p>Welcome to the Education section. Here you can find educational resources.</p>
    </div>
  );
};

export default EducationPage;


import React from 'react';

const HealthPage = () => {
  return (
    <div>
      <h1>Health</h1>
      <p>Welcome to the Health section. Here you can manage your health records.</p>
    </div>
  );
};

export default HealthPage;


import React from 'react';

const JobPage = () => {
  return (
    <div>
      <h1>Job</h1>
      <p>Welcome to the Job section. Here you can find job opportunities.</p>
    </div>
  );
};

export default JobPage;

