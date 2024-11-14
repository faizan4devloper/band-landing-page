const fetchDataForQuestion = async (question) => {
  const payload = {
    question_id: question.id,
    question: question.text,
  };

  try {
    const response = await axios.post(
      'dummy', // Replace with your actual API endpoint
      payload,
      { headers: { 'Content-Type': 'application/json' } }
    );

    console.log('API Response:', response.data)

    // Check if `response.data.body` is already an object or a JSON string
    const parsedResponse = typeof response.data.body === 'string'
      ? JSON.parse(response.data.body)
      : response.data.body;

    console.log(parsedResponse)

    const llmAnswer = parsedResponse.answer || '';
    const formattedAnswer = llmAnswer.split('-').map(line => line.trim()).filter(line => line);

    const factualData = parsedResponse.factualData && parsedResponse.factualData.startsWith("http")
      ? parsedResponse.factualData
      : 'No factual information available.';
      
    const citizenReview = parsedResponse.citizenReview && parsedResponse.citizenReview.startsWith("http")
      ? parsedResponse.citizenReview
      : 'No citizen experience information available.';

    const source = parsedResponse.source || 'No source available.'; // Add Source field

    return {
      question: question.text,
      answer: {
        textualResponse: formattedAnswer.length > 0 ? [...formattedAnswer, `Source: ${source}`] : ['No Answer Available'],
        factualData: factualData,
        citizenReview: citizenReview,
        contextual: parsedResponse.contextual || 'No contextual information available.',
      },
    };
  } catch (error) {
    console.error("Error fetching data for question:", error);
    return {
      question: question.text,
      answer: {
        textualResponse: ['No Answer Available'],
        factualData: 'No factual information available.',
        citizenReview: 'No citizen experience information available.',
        contextual: 'No contextual information available.',
      },
    };
  }
};










const renderResponsePoints = (responseArray) => {
  const arrayToRender = Array.isArray(responseArray) ? responseArray : [responseArray];
  return (
    <ul className={styles.responseList}>
      {arrayToRender.map((item, index) => (
        <li key={index} className={styles.responseItem}>
          {parseLinks(item)}
        </li>
      ))}
    </ul>
  );
};


<div className={styles.responseSection}>
  <p><strong>School Sources Insights:</strong></p>
  {renderResponsePoints(answerData.textualResponse)}
</div>
