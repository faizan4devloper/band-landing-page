function mapAssets(card, assetData) {
  const assetKey = card.title.replace(/\s+/g, '');
  const data = assetData[assetKey] ?? {};

  console.log('Mapping assets for:', assetKey, data);

  return {
    ...card,
    imageUrl: card.imageUrl ? images[card.imageUrl.split('.').pop()] : null,
    content: {
      ...card.content,
      solutionFlow: data.solutionFlow ?? null,
      demo: card.content.demo ? videos[card.content.demo.split('.').pop()] : null,
      techArchitecture: data.techArchitecture ?? null,
      descriptionFlow: data.description ?? null,
      benefitsFlow: data.benefits ?? null,
      adoption: data.adoption ?? null
    }
  };
}



{
  "imageUrl": "CitizenAdvisor",  // Make sure this matches the key in the `images` object if you use a key-based approach
  "title": "Citizen Advisor",
  "description": "An experience transformation from disconnected silos information to an intuitive, personalized revelations",
  "industry": "GOVT",
  "businessFunction": "Customer Experience",
  "content": {
    "description": ["description"],  // This should match the keys in `URLJson.json`
    "solutionFlow": ["solutionFlow"],  // This should match the keys in `URLJson.json`
    "demo": "CitizenAdvisorDemo",  // Make sure this key exists in `videos`
    "techArchitecture": ["techArchitecture"],  // This should match the keys in `URLJson.json`
    "benefits": ["benefits"],  // This should match the keys in `URLJson.json`
    "adoption": ["adoption"]  // This should match the keys in `URLJson.json`
  }
}
