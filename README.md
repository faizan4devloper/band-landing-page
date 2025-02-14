const parseMessages = (body) => {
  if (!body || typeof body !== 'string') {
    return {
      successMessages: [],
      unsuccessMessages: []
    };
  }

  try {
    const successMatch = body.includes('Successful Messages:')
      ? body.split('Successful Messages:')[1].split('Unsuccessful Messages:')[0]
      : '';

    const unsuccessMatch = body.includes('Unsuccessful Messages:')
      ? body.split('Unsuccessful Messages:')[1]
      : '';

    const extractMessages = (text) => {
      return text
        .trim()
        .split('\n')
        .map(msg => {
          try {
            const parsed = JSON.parse(msg); // Attempt to parse JSON objects
            if (typeof parsed === 'object' && parsed !== null) {
              return `${parsed.Missing_Information || 'Unknown Field'}: ${parsed.Explanation || 'No Explanation'}`;
            }
          } catch (e) {
            return msg; // Return as-is if not a JSON object
          }
        })
        .filter(msg => msg.trim() !== '');
    };

    return {
      successMessages: extractMessages(successMatch),
      unsuccessMessages: extractMessages(unsuccessMatch)
    };
  } catch (error) {
    console.error('Message parsing error:', error);
    return {
      successMessages: [],
      unsuccessMessages: []
    };
  }
};
