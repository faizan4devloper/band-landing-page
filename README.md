import React from 'react';
import styles from './FaqDropdown.module.css';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faChevronDown, faChevronUp } from '@fortawesome/free-solid-svg-icons';
import QuestionBlock from './QuestionBlock';

const FaqDropdown = ({ contentData, onQuestionSelect, selectedQuestion, selectedAnswer }) => {
  const [isFaqOpen, setIsFaqOpen] = React.useState(false); // Initially closed

  const toggleFaq = () => setIsFaqOpen(!isFaqOpen);

  const handleQuestionSelect = (question, answer) => {
    onQuestionSelect(question, answer);
    setIsFaqOpen(false); // Close dropdown after selection
  };

  return (
    <div className={styles.faqDropdown}>
      <div className={styles.dropdownHeader} onClick={toggleFaq}>
        <span>Frequently Asked Questions</span>
        <FontAwesomeIcon icon={isFaqOpen ? faChevronUp : faChevronDown} />
      </div>
      {isFaqOpen && (
        <div className={styles.dropdownContent}>
          {contentData.map((item, index) => (
            <div
              key={index}
              onClick={() => handleQuestionSelect(item.question, item.answer)}
              className={`${styles.questionBlock} ${selectedQuestion === item.question ? styles.active : ''}`} // Add active class
            >
              <span>{item.question}</span>
            </div>
          ))}
        </div>
      )}
      {selectedQuestion && (
        <QuestionBlock
          question={selectedQuestion}
          answerData={selectedAnswer}
        />
      )}
    </div>
  );
};

export default FaqDropdown;
