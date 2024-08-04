async function fetchAssets() {
  const proxyUrl = 'https://cors-anywhere.herokuapp.com/';
  const targetUrl = 'https://aiml-convai.s3.amazonaws.com/portal-slides/URLJson.json';
  
  try {
    const response = await axios.get(proxyUrl + targetUrl);
    return response.data;
  } catch (error) {
    console.error('Error fetching assets:', error);
    return {}; // Return an empty object or handle the error as needed
  }
}
