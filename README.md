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

    const Source = parsedResponse.Source || 'No source available.';

    // Mask the Source link into 'Click here'
    const maskedSource = Source.startsWith("http") 
      ? `<a href="${Source}" target="_blank" rel="noopener noreferrer">Click here</a>` 
      : 'No source available.';

    return {
      question: question.text,
      answer: {
        textualResponse: formattedAnswer.length > 0 ? [...formattedAnswer, `Source: ${maskedSource}`] : ['No Answer Available'],
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




const QuestionBlock = ({ question, answerData }) => {
  return (
    <div className={styles.questionBlock}>
      <h3>{question}</h3>
      <div>
        {answerData.textualResponse.map((line, index) => (
          <p key={index} dangerouslySetInnerHTML={{ __html: line }} />
        ))}
      </div>
    </div>
  );
};
