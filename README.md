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

    // Ensure URLs are formatted properly (e.g., adding .png extension if needed)
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

// Helper functions
const cleanUrl = (url) => {
  // Implement any necessary URL cleaning logic here.
  return url.trim();
};

const ensurePngExtension = (url) => {
  return url && !url.endsWith('.png') ? `${url}.png` : url;
};
