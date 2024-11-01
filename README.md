const fetchDataForQuestion = async ({ id, text }) => {
  try {
    const response = await axios.post(
      'dummy', // Replace with your actual endpoint
      { questionId: id, questionText: text },
      { headers: { 'Content-Type': 'application/json' }
    });

    const parsedResponse = JSON.parse(response.data.body);
    const llmAnswer = parsedResponse.answer;
    const formattedAnswer = llmAnswer ? llmAnswer.split('-').map(line => line.trim()).filter(line => line) : ['No Answer Available'];

    // Ensure URLs have the .png extension, if needed
    const factualData = ensurePngExtension(cleanUrl(parsedResponse.factualData));
    const citizenReview = ensurePngExtension(cleanUrl(parsedResponse.citizenReview));

    return {
      textualResponse: formattedAnswer,
      factualInfo: factualData || 'Factual information goes here.',
      citizenExperience: citizenReview || 'Citizen experience response goes here.',
      contextual: parsedResponse.contextual || 'Contextual information goes here.',
    };
  } catch (error) {
    console.error('Error fetching data:', error);
    return {
      textualResponse: ['No Answer Available'],
      factualInfo: null,
      citizenExperience: null,
      contextual: 'No contextual information available.',
    };
  }
};







const QuestionBlock = ({ question, answerData }) => {
  return (
    <div className={styles.questionBlock}>
      <div className={styles.question}>{question}</div>

      {/* Textual Response */}
      <div className={`${styles.responseSection} ${showFullTextual ? styles.expanded : ''}`}>
        <p><strong>Textual Response:</strong></p>
        {renderResponsePoints(answerData.textualResponse, showFullTextual)}
      </div>

      {/* Citizen Experience */}
      <div className={`${styles.responseSection} ${showFullCitizen ? styles.expanded : ''}`}>
        <p><strong>Citizen Experience:</strong></p>
        {renderImage(answerData.citizenExperience, "Citizen Experience Image")}
      </div>

      {/* Factual Info */}
      <div className={`${styles.responseSection} ${showFullFactual ? styles.expanded : ''}`}>
        <p><strong>Factual Info:</strong></p>
        {renderImage(answerData.factualInfo, "Factual Info Image")}
      </div>

      {/* Contextual */}
      <div className={`${styles.responseSection} ${showFullContextual ? styles.expanded : ''}`}>
        <p><strong>Contextual:</strong></p>
        {renderResponsePoints([answerData.contextual], showFullContextual)}
      </div>
    </div>
  );
};
