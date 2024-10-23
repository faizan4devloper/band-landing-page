const handleSubmit = async (e) => {
  e.preventDefault();

  if (input.trim() === '') return;

  // Add user's question to the conversation
  const userMessage = { type: 'user', text: input };
  setMessages([...messages, userMessage]);

  try {
    // Send POST request to the API
    const response = await axios.post('dummy', {
      question: input
    }, {
      headers: {
        'Content-Type': 'application/json'
      },
    });

    // Log the response for debugging
    console.log('API Response:', response.data);

    // Since the response body is a JSON string, we need to parse it
    const parsedBody = JSON.parse(response.data.body);

    // Extract the answer from the parsed response
    const answer = parsedBody.answer?.textual || 'No answer available for this question.';

    // Add chatbot's response to the conversation
    const botMessage = {
      type: 'bot',
      text: answer
    };
    setMessages((prevMessages) => [...prevMessages, botMessage]);

  } catch (error) {
    console.error('Error fetching data:', error);
    const errorMessage = { type: 'bot', text: 'Something went wrong. Please try again later.' };
    setMessages((prevMessages) => [...prevMessages, errorMessage]);
  }

  // Clear input field
  setInput('');
};
