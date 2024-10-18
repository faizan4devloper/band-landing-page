import React from 'react';
import { BrowserRouter as Router, Route, Routes } from 'react-router-dom';
import Personas from './components/Personas/Personas';
import EducationPage from './components/EducationPage';
import JobPage from './components/JobPage';
import HealthPage from './components/HealthPage';
import LoginPage from './components/Login/LoginPage';
import Header from './components/Header/Header';

function App() {
  return (
    <Router>
      <div className="App">
        <Header />
        <Routes>
          <Route path="/" element={<LoginPage />} />
          <Route path="/personas" element={<Personas />} />
          <Route path="/education" element={<EducationPage />} />
          <Route path="/job" element={<JobPage />} />
          <Route path="/health" element={<HealthPage />} />
        </Routes>
      </div>
    </Router>
  );
}

export default App;








import React from 'react';
import { useNavigate } from 'react-router-dom';
import styles from './Personas.module.css';

const personas = [
  {
    title: 'Education',
    description: 'Access educational resources and learning opportunities.',
    themeColor: '#4f46e5', // Example color for education
    route: '/education',
  },
  {
    title: 'Job',
    description: 'Find job opportunities and career development tools.',
    themeColor: '#f59e0b', // Example color for jobs
    route: '/job',
  },
  {
    title: 'Health',
    description: 'Manage health records and access health services.',
    themeColor: '#10b981', // Example color for health
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

const JobPage = () => {
  return (
    <div>
      <h1>Job</h1>
      <p>Welcome to the Job section. Here you can find job opportunities.</p>
    </div>
  );
};

export default JobPage;



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
