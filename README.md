import React from 'react';
import styles from './Personas.module.css';

const Personas = () => {
  return (
    <div className={styles.Personas}>
      <h1>Welcome to the Dashboard</h1>
      <p>This is your post-login page. You can add more content here.</p>
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
}

p {
  font-size: 1.2rem;
  color: #666;
}
