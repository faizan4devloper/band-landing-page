const fetchDataForQuestion = async ({ id, text }) => {
  try {
    const response = await axios.post(
      'dummy', // Replace with your actual endpoint
      { questionId: id, questionText: text },
      { headers: { 'Content-Type': 'application/json' } }
    );

    const parsedResponse = JSON.parse(response.data.body);
    const llmAnswer = parsedResponse.answer;
    const formattedAnswer = llmAnswer.split('-').map(line => line.trim()).filter(line => line);

    // Ensure URLs are processed correctly and check for "https" placeholder
    const factualInfo = (parsedResponse.factualInfo && parsedResponse.factualInfo !== "https") 
      ? ensurePngExtension(cleanUrl(parsedResponse.factualInfo)) 
      : null;
    const citizenExperience = (parsedResponse.citizenExperience && parsedResponse.citizenExperience !== "https") 
      ? ensurePngExtension(cleanUrl(parsedResponse.citizenExperience)) 
      : null;

    // Console logs with updated variable names
    console.log('Factual Info URL:', factualInfo);
    console.log('Citizen Experience URL:', citizenExperience);

    return {
      textualResponse: formattedAnswer.length > 0 ? formattedAnswer : ['No Answer Available'],
      factualInfo: factualInfo || 'Factual information goes here.',
      citizenExperience: citizenExperience || 'Citizen experience response goes here.',
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
