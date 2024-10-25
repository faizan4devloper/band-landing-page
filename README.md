const toggleAnswer = (index) => {
  setOpenQuestion(prevIndex => (prevIndex === index ? null : index)); // Toggle the selected question
  setOpenGridItems({
    textualResponse: true, // Textual response always expanded initially
  });
};



{contentData.map((item, index) => (
  <div key={index} className={styles.questionBlock}>
    <div className={styles.question} onClick={() => toggleAnswer(index)}>
      {item.question}
      <FontAwesomeIcon
        icon={openQuestion === index ? faChevronUp : faChevronDown}
        className={styles.chevronIcon}
      />
    </div>
    {openQuestion === index && (
      <div className={styles.gridAnswer}>
        <div
          className={`${styles.gridItem} ${openGridItems.textualResponse ? styles.expanded : styles.collapsed}`}
          onClick={() => toggleGridItem('textualResponse')}
        >
          <h3>Textual Response</h3>
          <FontAwesomeIcon
            icon={openGridItems.textualResponse ? faCaretUp : faCaretDown}
            className={styles.chevronIcon}
          />
          {openGridItems.textualResponse && (
            <ul className={styles.answerList}>
              {item.answer.textualResponse.map((line, idx) => (
                <li key={idx}>- {line}</li>
              ))}
            </ul>
          )}
        </div>

        <div
          className={`${styles.gridItem} ${openGridItems.citizenExperience ? styles.expanded : styles.collapsed}`}
          onClick={() => toggleGridItem('citizenExperience')}
        >
          <h3>Citizen Experience</h3>
          <FontAwesomeIcon
            icon={openGridItems.citizenExperience ? faCaretUp : faCaretDown}
            className={styles.chevronIcon}
          />
          {openGridItems.citizenExperience && (
            <ul className={styles.answerList}>
              <li>{item.answer.citizenExperience}</li>
            </ul>
          )}
        </div>

        <div
          className={`${styles.gridItem} ${openGridItems.factualInfo ? styles.expanded : styles.collapsed}`}
          onClick={() => toggleGridItem('factualInfo')}
        >
          <h3>Factual Info</h3>
          <FontAwesomeIcon
            icon={openGridItems.factualInfo ? faCaretUp : faCaretDown}
            className={styles.chevronIcon}
          />
          {openGridItems.factualInfo && (
            <ul className={styles.answerList}>
              <li>{item.answer.factualInfo}</li>
            </ul>
          )}
        </div>

        <div
          className={`${styles.gridItem} ${openGridItems.contextual ? styles.expanded : styles.collapsed}`}
          onClick={() => toggleGridItem('contextual')}
        >
          <h3>Contextual</h3>
          <FontAwesomeIcon
            icon={openGridItems.contextual ? faCaretUp : faCaretDown}
            className={styles.chevronIcon}
          />
          {openGridItems.contextual && (
            <ul className={styles.answerList}>
              <li>{item.answer.contextual}</li>
            </ul>
          )}
        </div>
      </div>
    )}
  </div>
))}
