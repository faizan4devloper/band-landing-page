{
  "descriptions": {
    "citizenDescription": "https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisor/description/CitizenAdvisorDescription.png",
    "smartRecruitDescription1": "https://aiml-convai.s3.amazonaws.com/portal-slides/smart-recruit/description/SmartRecruitDescription1.png",
    "smartRecruitDescription2": "https://aiml-convai.s3.amazonaws.com/portal-slides/smart-recruit/description/SmartRecruitDescription2.png"
  },
  "solutionFlows": {
    "CitizenAdvisorFlow1": "https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisor/solutionFlows/CitizenAdvisorFlow1.png",
    "CitizenAdvisorFlow2": "https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisor/solutionFlows/CitizenAdvisorFlow2.png",
    "CitizenAdvisorFlow3": "https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisor/solutionFlows/CitizenAdvisorFlow3.png",
    "CitizenAdvisorFlow4": "https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisor/solutionFlows/CitizenAdvisorFlow4.png",
    "CitizenAdvisorFlow5": "https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisor/solutionFlows/CitizenAdvisorFlow5.png"
  },
  "architectures": {
    "CitizenAdvisorArchitecture": "https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisor/technical-architecture/CitizenAdvisorArchitecture.png"
  },
  "solutionsBenefits": {
    "citizenBenefits": "https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisor/benefits/CitizenAdvisorBenefits.png"
  },
  "adoption": {
    "citizenAdoption": "https://aiml-convai.s3.amazonaws.com/portal-slides/citizenadvisor/industry-adoption/CitizenAdvisorAdoption.png"
  }
}




import URLJson from './URLJson.json'; // Import URLJson

function mapAssets(card) {
  return {
    ...card,
    imageUrl: card.imageUrl ? images[card.imageUrl] : null,
    content: {
      ...card.content,
      solutionFlow: Array.isArray(card.content.solutionFlow)
        ? card.content.solutionFlow.map(flow => URLJson.solutionFlows[flow] || null)
        : (URLJson.solutionFlows[card.content.solutionFlow] || []),
      demo: card.content.demo ? URLJson.videos[card.content.demo] : null,
      techArchitecture: card.content.techArchitecture
        ? URLJson.architectures[card.content.techArchitecture] || null
        : [],
      descriptionFlow: Array.isArray(card.content.description)
        ? card.content.description.map(desc => URLJson.descriptions[desc] || null)
        : (URLJson.descriptions[card.content.description] || []),
      benefitsFlow: card.content.benefits
        ? URLJson.solutionsBenefits[card.content.benefits] || null
        : [],
      adoptionFlow: card.content.adoption
        ? URLJson.adoption[card.content.adoption] || null
        : [],
    },
  };
}
