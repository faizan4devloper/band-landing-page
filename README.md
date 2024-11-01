const fetchDataForQuestion = async ({ id, text }) => {
  try {
    const response = await axios.post(
      'dummy', // Replace with your actual endpoint
      { questionId: id, questionText: text },
      { headers: { 'Content-Type': 'application/json' } }
    );

    // Log the full response for debugging purposes
    const parsedResponse = JSON.parse(response.data.body);
    console.log('Parsed Response:', parsedResponse);

    const llmAnswer = parsedResponse.answer || '';  // Default to empty string if undefined
    const formattedAnswer = llmAnswer ? llmAnswer.split('-').map(line => line.trim()).filter(line => line) : [];

    const factualData = parsedResponse.factualData && parsedResponse.factualData.startsWith("http")
      ? ensurePngExtension(cleanUrl(parsedResponse.factualData))
      : null;
    const citizenReview = parsedResponse.citizenReview && parsedResponse.citizenReview.startsWith("http")
      ? ensurePngExtension(cleanUrl(parsedResponse.citizenReview))
      : null;

    // Console logs to confirm the presence of URLs
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
