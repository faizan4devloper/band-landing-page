import image1 from "./components/Cards/card1.jpg";
import image2 from "./components/Cards/card2.jpg";
import image3 from "./components/Cards/card3.jpg";
import image4 from "./components/Cards/card4.jpg";
import image5 from "./components/Cards/card5.jpg";

import demoVideo1 from "./components/Sidebar/Icons/Email-EAR.mp4";
import demoVideo2 from "./components/Sidebar/Icons/SignatureExtraction.mp4";
import demoVideo3 from "./components/Sidebar/Icons/IntelligentAssist.mp4";
import demoVideo4 from "./components/Sidebar/Icons/CaseIntelligent.mp4";
import demoVideo5 from "./components/Sidebar/Icons/CodeGreat.mp4";

import solutionFlow1 from "./components/Sidebar/Icons/solutionFlowGraph1.png";
// import solutionFlow2 from "./components/Sidebar/Icons/solutionFlowGraph2.png";
import solutionFlow3 from "./components/Sidebar/Icons/solutionFlowGraph3.png";
import solutionFlow4 from "./components/Sidebar/Icons/solutionFlowGraph4.png";
// import solutionFlow4 from "./components/Icons/solutionFlow4.png";
// import solutionFlow5 from "./components/Icons/solutionFlow5.png";
import solutionFlow6 from "./components/Sidebar/Icons/solutionFlowGraph6.png";


import architecture1 from "./components/Sidebar/Icons/technicalArchitecture1.png";
import architecture2 from "./components/Sidebar/Icons/technicalArchitecture2.png";
import architecture3 from "./components/Sidebar/Icons/technicalArchitecture3.png";
import architecture4 from "./components/Sidebar/Icons/technicalArchitecture4.png";
import architecture5 from "./components/Sidebar/Icons/technicalArchitecture5.png";
import architecture6 from "./components/Sidebar/Icons/technicalArchitecture6.png";

export const cardsData = [
  {
    imageUrl: image1,
    title: "Email EAR",
    description: "Transform customer support process by automating email analysis, and providing crafted thoughtful response to incoming emails",
    content: {
      description: "Email EAR (Extract, Act and Respond) In today’s era, there are unprecedented ways of engaging the customer and this includes both traditional and digital channels. Unified experience across channels is key for customer delight.Despite the rise of alternative options, Email remains a preferred communication channel for customer support. Email provides unique benefits like maintaining a history of interactions, allowing customers to comprehensively explain issues, and enabling file attachments for additional context. However, the prioritization and queuing of emails often leads to delayed responses that create poor customer experiences. Our Gen AI-powered Email EAR (Extract, Act, and Respond) solution aims to transform the customer support process by automating the reading, analysis, and thoughtful responding to incoming emails. Specifically, our system can extract the core query, complaint, or issue within an email. It then understands what actions are required to resolve the customer's needs. Finally, it generates a user-friendly, detailed response explaining steps taken to address their questions or concerns.",
      solutionFlow: solutionFlow1,
      demo: demoVideo1,
      techArchitecture: architecture1,
      benefits: "This solution assists organizations in enhancing the customer experience for email responses. Automating responses to customer queries or integrating with applications to retrieve data or generate tickets facilitates completing the overall process without delays.  Timely and accurate responses help in increased self-service, reduced customer service inquiries, and higher customer satisfaction.  Human review of generated responses allows validation of customer feedback and enhancement of the knowledge base via feedback-driven updates.  Overall agent productivity stands to improve, as the solution can be seen as an agent assist that enables reviewing automatically generated responses and interacting with customers accordingly.  The adaptability of the solution allows it to be implemented across various verticals to address common customer pain points.",
 adoption: [
        { industry: "Financial", adoption: "Can help in answering finance product information, Loan eligibility, Product Recommendation as per the email context" },
        { industry: "Education", adoption: "Can help students to get response to their queries related to Admission process, Scholarship schemes, Enrollment process etc." },
        { industry: "Healthcare/Insurance", adoption: "Can help customers to understand their health insurance eligibility, claim processing, claim status etc." },
        { industry: "Retail", adoption: "Queries related to product exchange / return can be better handled" }
      ],      
    },
    
  },
