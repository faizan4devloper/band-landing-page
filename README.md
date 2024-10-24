const MainContent = () => {
  const [contentData, setContentData] = useState([]);
  const [loading, setLoading] = useState(true);
  const [openQuestion, setOpenQuestion] = useState(0);
  const [openGridItems, setOpenGridItems] = useState({
    textualResponse: true,
    citizenExperience: false,
    factualInfo: false,
    contextual: false,
  });

  const fetchData = async () => {
    try {
      const response = await axios.post(
        'dummy',
        {
          question:
            'what are the average class sizes and student-teacher ratios in the local schools react?',
        },
        {
          headers: {
            'Content-Type': 'application/json',
          },
        }
      );

      const parsedResponse = JSON.parse(response.data.body);
      const llmAnswer = parsedResponse.answer;

      const formattedData = [
        {
          question:
            'What are the average class sizes and student-teacher ratios in the local schools?',
          answer: {
            textualResponse: llmAnswer || '<ul><li>No data available for average class sizes</li></ul>',
            citizenExperience: '<ul><li>Citizen experience response 1</li><li>Experience response 2</li></ul>',
            factualInfo: '<ul><li>Factual info point 1</li><li>Factual info point 2</li></ul>',
            contextual: '<ol><li>Contextual info step 1</li><li>Contextual info step 2</li></ol>',
          },
        },
      ];

      setContentData(formattedData);
      setLoading(false);
    } catch (error) {
      console.error('Error fetching data:', error);
      setLoading(false);
    }
  };

  useEffect(() => {
    fetchData();
  }, []);

  const toggleAnswer = (index) => {
    if (openQuestion === index) {
      setOpenQuestion(null); // Collapse the question if clicked again
    } else {
      setOpenQuestion(index); // Open the new question
      setOpenGridItems({
        textualResponse: true,
        citizenExperience: false,
        factualInfo: false,
        contextual: false,
      });
    }
  };

  const toggleGridItem = (section) => {
    setOpenGridItems((prevState) => ({
      ...prevState,
      [section]: !prevState[section],
    }));
  };

  return (
    <div className={styles.mainContent}>
      {loading ? (
        <div className={styles.loaderWrapper}>
          <PropagateLoader color="rgb(15, 95, 220)" loading={loading} size={25} />
        </div>
      ) : Array.isArray(contentData) && contentData.length > 0 ? (
        contentData.map((item, index) => (
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
                  className={`${styles.gridItem} ${
                    openGridItems.textualResponse
                      ? styles.expanded
                      : styles.collapsed
                  }`}
                  onClick={() => toggleGridItem('textualResponse')}
                >
                  <h3>Textual Response</h3>
                  <FontAwesomeIcon
                    icon={
                      openGridItems.textualResponse ? faCaretUp : faCaretDown
                    }
                    className={styles.chevronIcon}
                  />
                  {openGridItems.textualResponse && (
                    <div
                      dangerouslySetInnerHTML={{
                        __html: item.answer.textualResponse,
                      }}
                    />
                  )}
                </div>

                <div
                  className={`${styles.gridItem} ${
                    openGridItems.citizenExperience
                      ? styles.expanded
                      : styles.collapsed
                  }`}
                  onClick={() => toggleGridItem('citizenExperience')}
                >
                  <h3>Citizen Experience</h3>
                  <FontAwesomeIcon
                    icon={
                      openGridItems.citizenExperience ? faCaretUp : faCaretDown
                    }
                    className={styles.chevronIcon}
                  />
                  {openGridItems.citizenExperience && (
                    <div
                      dangerouslySetInnerHTML={{
                        __html: item.answer.citizenExperience,
                      }}
                    />
                  )}
                </div>

                <div
                  className={`${styles.gridItem} ${
                    openGridItems.factualInfo ? styles.expanded : styles.collapsed
                  }`}
                  onClick={() => toggleGridItem('factualInfo')}
                >
                  <h3>Factual Info</h3>
                  <FontAwesomeIcon
                    icon={openGridItems.factualInfo ? faCaretUp : faCaretDown}
                    className={styles.chevronIcon}
                  />
                  {openGridItems.factualInfo && (
                    <div
                      dangerouslySetInnerHTML={{
                        __html: item.answer.factualInfo,
                      }}
                    />
                  )}
                </div>

                <div
                  className={`${styles.gridItem} ${
                    openGridItems.contextual ? styles.expanded : styles.collapsed
                  }`}
                  onClick={() => toggleGridItem('contextual')}
                >
                  <h3>Contextual</h3>
                  <FontAwesomeIcon
                    icon={openGridItems.contextual ? faCaretUp : faCaretDown}
                    className={styles.chevronIcon}
                  />
                  {openGridItems.contextual && (
                    <div
                      dangerouslySetInnerHTML={{
                        __html: item.answer.contextual,
                      }}
                    />
                  )}
                </div>
              </div>
            )}
          </div>
        ))
      ) : (
        <div>No data available</div>
      )}
    </div>
  );
};

export default MainContent;



.gridItem ul,
.gridItem ol {
  padding-left: 20px;
  margin: 10px 0;
}

.gridItem ul {
  list-style-type: disc; /* Bullet points for unordered lists */
}

.gridItem ol {
  list-style-type: decimal; /* Numbered lists for ordered lists */
}

.gridItem ul li,
.gridItem ol li {
  margin-bottom: 5px; /* Space between list items */
  color: #555;
}

.gridItem p, .gridItem li {
  font-size: 1em;
  line-height: 1.5;
  color: #333;
}
