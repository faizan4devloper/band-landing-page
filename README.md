import React, { useState } from 'react';
import styles from './GridAnswer.module.css';
import GridItem from './GridItem';

const GridAnswer = ({ answerData }) => {
  const [openGridItems, setOpenGridItems] = useState({
    textualResponse: true,
  });

  const toggleGridItem = (section) => {
    setOpenGridItems(prevState => ({
      ...prevState,
      [section]: !prevState[section],
    }));
  };

  return (
    <div className={styles.gridAnswer}>
      <GridItem
        title="Textual Response"
        isOpen={openGridItems.textualResponse}
        toggle={() => toggleGridItem('textualResponse')}
        content={answerData.textualResponse}
      />
      <GridItem
        title="Citizen Experience"
        isOpen={openGridItems.citizenExperience}
        toggle={() => toggleGridItem('citizenExperience')}
        content={[answerData.citizenExperience]}
      />
      <GridItem
        title="Factual Info"
        isOpen={openGridItems.factualInfo}
        toggle={() => toggleGridItem('factualInfo')}
        content={[answerData.factualInfo]}
      />
      <GridItem
        title="Contextual"
        isOpen={openGridItems.contextual}
        toggle={() => toggleGridItem('contextual')}
        content={[answerData.contextual]}
      />
    </div>
  );
};

export default GridAnswer;


.gridAnswer {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  max-height: 300px;
  overflow-y: auto;
  margin-top: 10px;
  gap: 20px;
  background-color: #eff0f7;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}


.answerList {
  padding-left: 20px;
  list-style: none;
  font-size: 1em;
  color: #555;
}

.answerList li {
  margin-bottom: 10px;
  line-height: 1.6;
  font-size: .9rem;
  font-weight: 500;
  color: #333;
}

/* Custom scrollbar */
.gridAnswer::-webkit-scrollbar{
  width: 8px;
}

.gridAnswer::-webkit-scrollbar-track {
  background: #f1f1f1;
}

.gridAnswer::-webkit-scrollbar-thumb {
  background-color: #888;
  border-radius: 8px;
  border: 2px solid #f1f1f1;
}

.gridAnswer::-webkit-scrollbar-thumb:hover {
  background-color: #555;
}
