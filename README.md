<div className={styles.gridAnswer}>
                {/* Textual Response expanded by default */}
                <div
  className={`${styles.gridItem} ${
    openGridItems.textualResponse ? styles.expanded : styles.collapsed
  }`}
  onClick={() => toggleGridItem('textualResponse')}
>
  <h3>Textual Response</h3>
  <FontAwesomeIcon
                icon={openGridItems.textualResponse ? faChevronUp : faChevronDown}
                className={styles.chevronIcon}
              />
  {openGridItems.textualResponse && <p>{item.answer.textualResponse}</p>}
</div>

<div
  className={`${styles.gridItem} ${
    openGridItems.citizenExperience ? styles.expanded : styles.collapsed
  }`}
  onClick={() => toggleGridItem('citizenExperience')}
>
  <h3>Citizen Experience</h3>
  <FontAwesomeIcon
                icon={openGridItems.textualResponse ? faChevronUp : faChevronDown}
                className={styles.chevronIcon}
              />
  {openGridItems.citizenExperience && <p>{item.answer.citizenExperience}</p>}
</div>

<div
  className={`${styles.gridItem} ${
    openGridItems.factualInfo ? styles.expanded : styles.collapsed
  }`}
  onClick={() => toggleGridItem('factualInfo')}
>
  <h3>Factual Info</h3>
  <FontAwesomeIcon
                icon={openGridItems.textualResponse ? faChevronUp : faChevronDown}
                className={styles.chevronIcon}
              />
  {openGridItems.factualInfo && <p>{item.answer.factualInfo}</p>}
</div>

<div
  className={`${styles.gridItem} ${
    openGridItems.contextual ? styles.expanded : styles.collapsed
  }`}
  onClick={() => toggleGridItem('contextual')}
>
  <h3>Contextual</h3>
  <FontAwesomeIcon
                icon={openGridItems.textualResponse ? faChevronUp : faChevronDown}
                className={styles.chevronIcon}
              />
  {openGridItems.contextual && <p>{item.answer.contextual}</p>}
</div>
              </div>
