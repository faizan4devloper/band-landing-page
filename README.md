function mapAssets(card) {
  return {
    ...card,
    imageUrl: card.imageUrl ? images[card.imageUrl.split('.').pop()] : null,
    content: {
      ...card.content,
      solutionFlow: Array.isArray(card.content.solutionFlow)
        ? card.content.solutionFlow.map(flow => typeof flow === 'string' ? solutionFlows[flow.split('.').pop()] : null)
        : [],
      demo: card.content.demo ? videos[card.content.demo.split('.').pop()] : null,
      techArchitecture: Array.isArray(card.content.techArchitecture)
        ? card.content.techArchitecture.map(arch => typeof arch === 'string' ? architectures[arch.split('.').pop()] : null)
        : [],
      descriptionFlow: Array.isArray(card.content.description)
        ? card.content.description.map(desc => typeof desc === 'string' ? descriptions[desc.split('.').pop()] : null)
        : [],
      benefitsFlow: Array.isArray(card.content.benefits)
        ? card.content.benefits.map(benefit => typeof benefit === 'string' ? solutionsBenefits[benefit.split('.').pop()] : null)
        : [],
      adoptionFlow: Array.isArray(card.content.adoption)
        ? card.content.adoption.map(adopt => typeof adopt === 'string' ? adoption[adopt.split('.').pop()] : null)
        : [],
    },
  };
}


function mapAssets(card) {
  return {
    ...card,
    imageUrl: card.imageUrl ? images[card.imageUrl.split('.').pop()] : null,
    content: {
      ...card.content,
      solutionFlow: Array.isArray(card.content.solutionFlow)
        ? card.content.solutionFlow.map(flow => solutionFlows[flow.split('.').pop()])
        : [],
      demo: card.content.demo ? videos[card.content.demo.split('.').pop()] : null,
      techArchitecture: Array.isArray(card.content.techArchitecture)
        ? card.content.techArchitecture.map(arch => architectures[arch.split('.').pop()])
        : [],
      descriptionFlow: Array.isArray(card.content.description)
        ? card.content.description.map(desc => descriptions[desc.split('.').pop()])
        : [],
      benefitsFlow: Array.isArray(card.content.benefits)
        ? card.content.benefits.map(benefit => solutionsBenefits[benefit.split('.').pop()])
        : [],
      adoptionFlow: Array.isArray(card.content.adoption)
        ? card.content.adoption.map(adopt => adoption[adopt.split('.').pop()])
        : [],
    },
  };
}

Uncaught TypeError: adopt.split is not a function
    at bundle.js:sourcemap:3563:110
    at Array.map (<anonymous>)
    at mapAssets (bundle.js:sourcemap:3563:82)
    at ./src/data.js (bundle.js:sourcemap:3567:20)
    at options.factory (bundle.js:sourcemap:86459:31)
    at __webpack_require__ (bundle.js:sourcemap:85880:32)
    at fn (bundle.js:sourcemap:86117:21)
    at ./src/components/Sidebar/SideBarPage.js (bundle.js:sourcemap:3254:63)
    at options.factory (bundle.js:sourcemap:86459:31)
    at __webpack_require__ (bundle.js:sourcemap:85880:32)

function mapAssets(card) {
  return {
    ...card,
    imageUrl: card.imageUrl ? images[card.imageUrl.split('.').pop()] : null,
    content: {
      ...card.content,
      solutionFlow: Array.isArray(card.content.solutionFlow)
        ? card.content.solutionFlow.map(flow => solutionFlows[flow.split('.').pop()])
        : [],
      demo: card.content.demo ? videos[card.content.demo.split('.').pop()] : null,
      techArchitecture: Array.isArray(card.content.techArchitecture)
        ? card.content.techArchitecture.map(arch => architectures[arch.split('.').pop()])
        : [],
      descriptionFlow: Array.isArray(card.content.description)
        ? card.content.description.map(desc => descriptions[desc.split('.').pop()])
        : [],
      benefitsFlow: Array.isArray(card.content.benefits)
        ? card.content.benefits.map(benefit => solutionsBenefits[benefit.split('.').pop()])
        : [],
      adoptionFlow: Array.isArray(card.content.adoption)
        ? card.content.adoption.map(adopt => adoption[adopt.split('.').pop()])
        : [],
    },
  };
}




const MainContent = ({ activeTab, content }) => {
  const [maximizedImage, setMaximizedImage] = useState(null);
  const [currentSlide, setCurrentSlide] = useState(0);

  const toggleMaximize = (imageSrc) => {
    setMaximizedImage(maximizedImage === imageSrc ? null : imageSrc);
  };

  const renderCarousel = (images) => (
    <div className={styles.carouselContainer}>
      <Carousel
        showArrows={true}
        showIndicators={true}
        showThumbs={true}
        selectedItem={currentSlide}
        onChange={(index) => setCurrentSlide(index)}
        className={styles.customCarousel}
      >
        {images.map((image, index) => (
          <div key={index}>
            <img
              src={image}
              alt={`Slide ${index + 1}`}
              className={maximizedImage === image ? styles.maximized : ""}
              onClick={() => toggleMaximize(image)}
            />
          </div>
        ))}
      </Carousel>
    </div>
  );

  const renderImageOrCarousel = (images) => (
    images.length > 1 ? renderCarousel(images) : (
      <img
        src={images[0]}
        alt="Single Image"
        className={maximizedImage === images[0] ? styles.maximized : ""}
        onClick={() => toggleMaximize(images[0])}
      />
    )
  );

  if (!content || !content.description) {
    return <div className={styles.mainContent}>Description not available</div>;
  }

  const contentMap = {
    description: (
      <div className={styles.description}>
        {renderImageOrCarousel(content.descriptionFlow)}
      </div>
    ),
    solutionFlow: (
      <div className={styles.solution}>
        {renderImageOrCarousel(content.solutionFlow)}
      </div>
    ),
    demo: (
      <div className={styles.demo}>
        <Video src={content.demo} />
      </div>
    ),
    techArchitecture: (
      <div className={styles.architecture}>
        {renderImageOrCarousel(content.techArchitecture)}
      </div>
    ),
    benefits: (
      <div className={styles.benefits}>
        {renderImageOrCarousel(content.benefitsFlow)}
      </div>
    ),
    adoption: (
      <div className={styles.adoption}>
        {renderImageOrCarousel(content.adoptionFlow)}
      </div>
    ),
  };

  return (
    <div className={styles.mainContent}>
      {contentMap[activeTab] || <div>Content not available</div>}
      {maximizedImage && (
        <div className={styles.overlay} onClick={() => setMaximizedImage(null)}>
          <FontAwesomeIcon icon={faTimes} className={styles.closeIcon} onClick={() => setMaximizedImage(null)} />
          <img src={maximizedImage} alt="Maximized view" className={styles.maximized} />
        </div>
      )}
    </div>
  );
};

export default MainContent;






// Images
export const images = {
  IntelligentAss: require('./components/Cards/CardsImages/card3.jpg'),
  EmailEAR: require('./components/Cards/CardsImages/card1.jpg'),
  CaseIntelligence: require('./components/Cards/CardsImages/card4.jpg'),
  SmartRecruit: require('./components/Cards/CardsImages/card8.jpg'),
  IAssureClaim: require('./components/Cards/CardsImages/card9.jpg'),
  AssistantEV: require('./components/Cards/CardsImages/card10.jpg'),
  CitizenAdvisor: require('./components/Cards/CardsImages/dummy2.jpg'),
  FinanceCompetitor: require('./components/Cards/CardsImages/card82.jpg'),
  Signature: require('./components/Cards/CardsImages/card2.jpg'),
  AIForce: require('./components/Cards/CardsImages/card19.jpg'),
  APICase: require('./components/Cards/CardsImages/card13.jpg'),
  AMSSupport: require('./components/Cards/CardsImages/AUTOMATION.jpg'),
  SOP: require('./components/Cards/CardsImages/SOP.jpg'),
  CodeGReat: require('./components/Cards/CardsImages/card5.jpg'),
  AAIG: require('./components/Cards/CardsImages/card16.jpg'),
  ResponsibleGen: require('./components/Cards/CardsImages/card17.jpg'),
  GraphData: require('./components/Cards/CardsImages/card18.jpg'),
  PredictiveAsset: require('./components/Cards/CardsImages/card11.jpg'),
};

// Videos
export const videos = {
  EmailEARDemo: 'https://aiml-convai.s3.amazonaws.com/demovideos/Email-EAR_Demo_new.mp4',
  SignatureExtractionDemo: 'https://aiml-convai.s3.amazonaws.com/demovideos/Sign_Verification_New.mp4',
  IntelligentAssistDemo: 'https://aiml-convai.s3.amazonaws.com/demovideos/Intelligent_Assist-QnA_DemoVideo_new.mp4',
  CaseIntelligenceDemo: 'https://aiml-convai.s3.amazonaws.com/demovideos/Case-Intelligence_demo.mp4',
  CodeGReatDemo: 'https://aiml-convai.s3.amazonaws.com/demovideos/CodeGreat_Demo_new.mp4',
  SmartRecruitDemo: 'https://aiml-convai.s3.amazonaws.com/demovideos/SmartRecruit_IvAssist_Demo.mp4',
};

// Solution Flows
export const solutionFlows = {
  EmailEarFlow: require('./components/Sidebar/Icons/EmailEarFlowGraph.png'),
  IntelligentAssistFlow: require('./components/Sidebar/Icons/IntelligentAssistFlowGraph.png'),
  CaseIntelligenceFlow: require('./components/Sidebar/Icons/CaseIntelligenceFlowGraph.png'),
  SmartRecruitFlow: require('./components/Sidebar/Icons/SmartRecruitFlowGraph.png'),
  IAssureClaimFlow: require('./components/Sidebar/Icons/IAssureClaimFlowGraph.png'),
  CitizenAdvisorFlow1: require('./components/Sidebar/Icons/Slide1.png'),
  CitizenAdvisorFlow2: require('./components/Sidebar/Icons/Slide2.png'),
  CitizenAdvisorFlow3: require('./components/Sidebar/Icons/Slide3.png'),
  CitizenAdvisorFlow4: require('./components/Sidebar/Icons/Slide4.png'),
  SmartRecruitSolutionFlow1: require('./components/Sidebar/Icons/SmartRecruitSolutionFlow1.png'),
  SmartRecruitSolutionFlow2: require('./components/Sidebar/Icons/SmartRecruitSolutionFlow2.png'),
};

// Technical Architectures
export const architectures = {
  IntelligentAssistArchitecture: require('./components/Sidebar/Icons/IntelligentAssistarchitecture.png'),
  EmailEARArchitecture: require('./components/Sidebar/Icons/EmailEARarchitecture.png'),
  CaseIntelligenceArchitecture: require('./components/Sidebar/Icons/CaseIntelligencearchitecture.png'),
  SmartRecruitArchitecture: require('./components/Sidebar/Icons/SmartRecruitArchitecture.png'),
  AssistantEvArchitecture: require('./components/Sidebar/Icons/AssistantEvachitecture.png'),
  IAssureClaimArchitecture: require('./components/Sidebar/Icons/IAssureClaimarchitecture.png'),
  AIForceArchitecture: require('./components/Sidebar/Icons/AIForcearchitecture.png'),
  CitizenAdvisorArchitecture: require('./components/Sidebar/Icons/Slide7.png'),
};

// Descriptions
export const descriptions = {
  citizenDescription: require('./components/Sidebar/Icons/CitizenAdvisorDesc.png'),
  smartRecruitDescription: require('./components/Sidebar/Icons/SmartRecruitDescription1.png'),
};

// Solutions Benefits
export const solutionsBenefits = {
  citizenBenefits: require('./components/Sidebar/Icons/CitizenAdvisorBenefits.png'),
  smartRecruitBenefits: require('./components/Sidebar/Icons/SmartRecruitBenefits.png'),
};

// Adoption
export const adoption = {
  citizenAdoption: require('./components/Sidebar/Icons/CitizenAdoption.png'),
  smartRecruitAdoption: require('./components/Sidebar/Icons/SmartRecruitAdoption.png'),
};



{
  "imageUrl": "SmartRecruit",
  "title": "Smart Recruit",
  "description": "A powerful tool for streamlining the recruitment process using AI and automation.",
  "industry": "HR",
  "businessFunction": "Recruitment",
  "content": {
    "description": [
      "smartRecruitDescription1",
      "smartRecruitDescription2"
    ],
    "solutionFlow": [
      "SmartRecruitSolutionFlow1",
      "SmartRecruitSolutionFlow2"
    ],
    "demo": "SmartRecruitDemo",
    "techArchitecture": "SmartRecruitArchitecture",
    "benefits": "smartRecruitBenefits",
    "adoption": "smartRecruitAdoption"
  }
}
