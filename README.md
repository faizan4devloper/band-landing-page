// fetchAssetUrls.js
export const fetchAssetUrls = async () => {
  try {
    const response = await fetch('https://aiml-convai.s3.amazonaws.com/portal-slides/urldata.json', {
      method: 'GET',
      headers: {
        'Content-Type': 'application/json',
      },
      mode: 'cors', // This allows cross-origin requests if the server supports it
    });
    
    if (!response.ok) {
      throw new Error('Network response was not ok');
    }

    return await response.json();
  } catch (error) {
    console.error('Error fetching asset URLs:', error);
    throw error;
  }
};
