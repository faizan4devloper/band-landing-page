const fetchData = async () => {
  try {
    const response = await axios.post('dummy', {
      question: 'what are the average class sizes and student-teacher ratios in the local schools react?'
    }, {
      headers: {
        'Content-Type': 'application/json'
      },
    });

    console.log('API Response:', response.data); // Check the structure of the response

    // Parse the response correctly
    const parsedResponse = JSON.parse(response.data.body);
    const llmAnswer = parsedResponse.answer; // This is where the answer is retrieved

    const formattedData = [
      {
        question: 'What are the average class sizes and student-teacher ratios in the local schools?',
        answer: llmAnswer || 'No Answer Available' // Fallback in case answer is not available
      }
    ];
    
    setContentData(formattedData);
  } catch (error) {
    console.error('Error fetching data:', error);
  }
};
