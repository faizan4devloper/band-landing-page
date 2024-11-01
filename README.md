const fetchDataForQuestion = async ({ id, text }) => {
  try {
    const response = await axios.post(
      'dummy', // Replace with your actual endpoint
      { questionId: id, questionText: text },
      { headers: { 'Content-Type': 'application/json' } }
    );

    const parsedResponse = JSON.parse(response.data.body);
    console.log('Parsed Response:', parsedResponse);

    const llmAnswer = parsedResponse.answer || '';
    const formattedAnswer = llmAnswer.split('-').map(line => line.trim()).filter(line => line);

    // Safely get factualData and citizenReview URLs
    const factualData = parsedResponse.factualData && parsedResponse.factualData.startsWith("http")
      ? parsedResponse.factualData
      : null;
    
    const citizenReview = parsedResponse.citizenReview && parsedResponse.citizenReview.startsWith("http")
      ? parsedResponse.citizenReview
      : null;

    console.log('Factual Data URL:', factualData);
    console.log('Citizen Review URL:', citizenReview);

    return {
      textualResponse: formattedAnswer.length > 0 ? formattedAnswer : ['No Answer Available'],
      factualData: factualData || 'No factual information available.',
      citizenReview: citizenReview || 'No citizen experience information available.',
      contextual: parsedResponse.contextual || 'No contextual information available.',
    };
  } catch (error) {
    console.error('Error fetching data:', error);
    return {
      textualResponse: ['No Answer Available'],
      factualData: null,
      citizenReview: null,
      contextual: 'No contextual information available.',
    };
  }
};
