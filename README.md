useEffect(() => {
  const fetchHistoricClaimDetails = async () => {
    if (!claimType) return;

    setLoading(true);
    try {
      const response = await axios.post(
        'https://history',
        { claimtype: claimType }
      );

      console.log('Raw Response:', response.data);

      let parsedBody;

      try {
        let responseBody = response.data.body;

        // Ensure it's a string before proceeding
        if (typeof responseBody !== 'string') {
          parsedBody = responseBody;
        } else {
          // Attempt to fix unescaped characters by using JSON.stringify and parsing again
          responseBody = responseBody.replace(/[\u0000-\u001F]+/g, ''); // Remove control characters

          // First parsing attempt
          parsedBody = JSON.parse(responseBody);

          // Handle double-encoded JSON
          if (typeof parsedBody === 'string') {
            parsedBody = JSON.parse(parsedBody);
          }
        }
      } catch (error) {
        console.error('JSON Parsing Error:', error);
        throw new Error('Invalid data format');
      }

      if (!parsedBody || typeof parsedBody !== 'object') {
        throw new Error('Invalid data format');
      }

      console.log('Parsed Historic Data:', parsedBody);
      setHistoricData(parsedBody);
      setLoading(false);
    } catch (err) {
      console.error('Error details:', {
        message: err.message,
        response: err.response ? err.response.data : 'No response',
        stack: err.stack
      });
      setError(err.message);
      setLoading(false);
    }
  };

  fetchHistoricClaimDetails();
}, [claimType]);



Insights.js:44 JSON Parsing Error: SyntaxError: Bad control character in string literal in JSON at position 606 (line 13 column 106)
    at JSON.parse (<anonymous>)
    at fetchHistoricClaimDetails (Insights.js:41:1)

Insights.js:57 Error details: 
{message: 'Invalid data format', response: 'No response', stack: 'Error: Invalid data format\n    at fetchHistoricCla…east-1.amazonaws.com/static/js/bundle.js:4969:17)'}
