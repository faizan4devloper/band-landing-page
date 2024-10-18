import React from 'react';
import styles from './Personas.module.css';

const personas = [
  {
    title: 'Education',
    description: 'Access educational resources and learning opportunities.',
    themeColor: '#4f46e5', // Example color for education
  },
  {
    title: 'Job',
    description: 'Find job opportunities and career development tools.',
    themeColor: '#f59e0b', // Example color for jobs
  },
  {
    title: 'Health',
    description: 'Manage health records and access health services.',
    themeColor: '#10b981', // Example color for health
  },
];

const Personas = () => {
  return (
    <div className={styles.Personas}>
      <h1>Your Personas</h1>
      <div className={styles.personaCards}>
        {personas.map((persona, index) => (
          <div
            key={index}
            className={styles.personaCard}
            style={{ backgroundColor: persona.themeColor }}
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
  background-color: #f4f4f9;
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
  color: white;
  text-align: center;
  box-shadow: 0 10px 15px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s ease;
}

.personaCard:hover {
  transform: translateY(-10px);
}

.personaCard h2 {
  font-size: 1.5rem;
  margin-bottom: 1rem;
}

.personaCard p {
  font-size: 1rem;
}
