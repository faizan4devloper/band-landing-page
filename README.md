import image1 from "./components/Cards/card1.jpg";
import image2 from "./components/Cards/card2.jpg";
import image3 from "./components/Cards/card3.jpg";
import image4 from "./components/Cards/card4.jpg";
import image5 from "./components/Cards/card5.jpg";

import demoVideo1 from "./components/Sidebar/Icons/demoVideo1.mp4";
import demoVideo2 from "./components/Sidebar/Icons/demoVideo2.mp4";
import demoVideo3 from "./components/Sidebar/Icons/demoVideo3.mp4";
import demoVideo4 from "./components/Sidebar/Icons/demoVideo4.mp4";
import demoVideo5 from "./components/Sidebar/Icons/demoVideo5.mp4";

import solutionFlow1 from "./components/Sidebar/Icons/solutionFlowGraph.png";
// import solutionFlow2 from "./components/Icons/solutionFlow2.png";
// import solutionFlow3 from "./components/Icons/solutionFlow3.png";
// import solutionFlow4 from "./components/Icons/solutionFlow4.png";
// import solutionFlow5 from "./components/Icons/solutionFlow5.png";

import architecture1 from "./components/Sidebar/Icons/technicalArchitecture.png";
// import architecture2 from "./components/Icons/architecture2.png";
// import architecture3 from "./components/Icons/architecture3.png";
// import architecture4 from "./components/Icons/architecture4.png";
// import architecture5 from "./components/Icons/architecture5.png";

export const cardsData = [
  {
    imageUrl: image1,
    title: "Solution 1",
    description: "Description for Solution 1.",
    content: {
      description: "Detailed description for Solution 1.",
      solutionFlow: solutionFlow1,
      demo: demoVideo1,
      techArchitecture: architecture1,
      benefits: "Benefits for Solution 1.",
    },
  },
  {
    imageUrl: image2,
    title: "Solution 2",
    description: "Description for Solution 2.",
    content: {
      description: "Detailed description for Solution 2.",
      solutionFlow: "solutionFlow2",
      demo: demoVideo2,
      techArchitecture: "architecture2",
      benefits: "Benefits for Solution 2.",
    },
  },
  {
    imageUrl: image3,
    title: "Solution 3",
    description: "Description for Solution 3.",
    content: {
      description: "Detailed description for Solution 3.",
      solutionFlow: "solutionFlow3",
      demo: demoVideo3,
      techArchitecture: "architecture3",
      benefits: "Benefits for Solution 3.",
    },
  },
  {
    imageUrl: image4,
    title: "Solution 4",
    description: "Description for Solution 4.",
    content: {
      description: "Detailed description for Solution 4.",
      solutionFlow: "solutionFlow4",
      demo: demoVideo4,
      techArchitecture: "architecture4",
      benefits: "Benefits for Solution 4.",
    },
  },
  {
    imageUrl: image5,
    title: "Solution 5",
    description: "Description for Solution 5.",
    content: {
      description: "Detailed description for Solution 5.",
      solutionFlow: "solutionFlow5",
      demo: demoVideo5,
      techArchitecture: "architecture5",
      benefits: "Benefits for Solution 5.",
    },
  },
];




import React, { useState, useEffect } from "react";
import { useLocation } from "react-router-dom";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faArrowLeft } from "@fortawesome/free-solid-svg-icons";
import Header from "../Header/Header";
import Footer from "../Footer/Footer";
import SideBar from "./SideBar";
import MainContent from "./MainContent";
import styles from "./SideBarPage.module.css"; // Import the module.css file for styling
import { cardsData } from "../../data"; // Import the card data

const SideBarPage = () => {
  const [activeTab, setActiveTab] = useState("description");
  const [cardContent, setCardContent] = useState({});
  const location = useLocation();

  useEffect(() => {
    // Extract card title from query parameter
    const params = new URLSearchParams(location.search);
    const title = params.get("title");
    if (title) {
      const card = cardsData.find((card) => card.title === title);
      if (card) {
        setCardContent(card.content);
      }
    }
  }, [location.search]);

  const handleTabChange = (tabId) => {
    setActiveTab(tabId);
  };

  // Function to handle clicking on the back button
  const handleBackButtonClick = () => {
    window.history.back(); // Go back to the previous page
  };

  return (
    <div className={styles.sideBarPage}>
      <Header />
      <div className={styles.header}>
        <button onClick={handleBackButtonClick} className={styles.backButton}>
          <FontAwesomeIcon icon={faArrowLeft} />
        </button>
        <div className={styles.cardTitle}>{cardContent.title}</div>
      </div>
      <div className={styles.contentWrapper}>
        <SideBar activeTab={activeTab} handleTabChange={handleTabChange} />
        <MainContent activeTab={activeTab} content={cardContent} />
      </div>
      <Footer />
    </div>
  );
};

export default SideBarPage;

import React from "react";
import { BrowserRouter as Router, Routes, Route } from "react-router-dom";
import Header from "./components/Header/Header";
import MyCarousel from "./components/Carousel/MyCarousel";
import Cards from "./components/Cards/Cards";
import Footer from "./components/Footer/Footer";
import styles from "./App.module.css";
import SideBarPage from "./components/Sidebar/SideBarPage";
import { cardsData } from "./data"; // Import card data from a separate file

const App = () => {
  const [bigIndex, setBigIndex] = React.useState(null);

  const toggleSize = (index) => {
    setBigIndex(index === bigIndex ? null : index);
  };

  const handleClickLeft = () => {
    if (bigIndex === null || bigIndex === 0) {
      setBigIndex(cardsData.length - 1);
    } else {
      setBigIndex(bigIndex - 1);
    }
  };

  const handleClickRight = () => {
    if (bigIndex === null || bigIndex === cardsData.length - 1) {
      setBigIndex(0);
    } else {
      setBigIndex(bigIndex + 1);
    }
  };

  return (
    <Router>
      <div className={styles.app}>
        <Header />
        <Routes>
          <Route
            path="/"
            element={
              <Home
                cardsData={cardsData}
                handleClickLeft={handleClickLeft}
                handleClickRight={handleClickRight}
                bigIndex={bigIndex}
                toggleSize={toggleSize}
              />
            }
          />
          <Route path="/dashboard" element={<SideBarPage />} />
        </Routes>
        <Footer />
      </div>
    </Router>
  );
};

const Home = ({
  cardsData,
  handleClickLeft,
  handleClickRight,
  bigIndex,
  toggleSize,
}) => {
  return (
    <>
      <MyCarousel />
      <div className={styles.cardsContainer}>
        <span
          className={`${styles.arrow} ${styles.leftArrow}`}
          onClick={handleClickLeft}
        >
          &#129104;
        </span>
        {cardsData.map((card, index) => (
          <Cards
            key={index}
            imageUrl={card.imageUrl}
            title={card.title}
            description={card.description}
            isBig={index === bigIndex}
            toggleSize={() => toggleSize(index)}
          />
        ))}
        <span
          className={`${styles.arrow} ${styles.rightArrow}`}
          onClick={handleClickRight}
        >
          &#129106;
        </span>
      </div>
    </>
  );
};

export default App;





import React from "react";
import styles from "./MainContent.module.css";
import Video from "./Video";

const MainContent = ({ activeTab, content }) => {
  const contentMap = {
    description: (
      <div>
        <h2>Description</h2>
        <p>{content.description}</p>
      </div>
    ),
    solutionFlow: (
      <div>
        <h2>Solution Flow</h2>
        <img src={content.solutionFlow} alt="Solution Flow" />
        <p>{content.solutionFlow}</p>
      </div>
    ),
    demo: (
      <div>
        <h2>Demo</h2>
        <Video src={content.demo} />
      </div>
    ),
    techArchitecture: (
      <div>
        <h2>Technical Architecture</h2>
        <img src={content.techArchitecture} alt="Technical Architecture" />
        <p>{content.techArchitecture}</p>
      </div>
    ),
    benefits: (
      <div>
        <h2>Benefits</h2>
        <p>{content.benefits}</p>
      </div>
    ),
  };

  return <div className={styles.mainContent}>{contentMap[activeTab]}</div>;
};

export default MainContent;






import React, { useState, useEffect } from "react";
import { useLocation } from "react-router-dom";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faArrowLeft } from "@fortawesome/free-solid-svg-icons";
import Header from "../Header/Header";
import Footer from "../Footer/Footer";
import SideBar from "./SideBar";
import MainContent from "./MainContent";
import styles from "./SideBarPage.module.css"; // Import the module.css file for styling
import { cardsData } from "../../data"; // Import cardsData from the data file

const SideBarPage = () => {
  const [activeTab, setActiveTab] = useState("description");
  const [cardContent, setCardContent] = useState({});
  const [cardTitle, setCardTitle] = useState("");
  const location = useLocation();

  useEffect(() => {
    // Extract card title from query parameter
    const params = new URLSearchParams(location.search);
    const title = params.get("title");
    if (title) {
      setCardTitle(title);
      const card = cardsData.find((c) => c.title === title);
      if (card) {
        setCardContent(card.content);
      }
    }
  }, [location.search]);

  const handleTabChange = (tabId) => {
    setActiveTab(tabId);
  };

  // Function to handle clicking on the back button
  const handleBackButtonClick = () => {
    window.history.back(); // Go back to the previous page
  };

  return (
    <div className={styles.sideBarPage}>
      <Header />
      <div className={styles.header}>
        <button onClick={handleBackButtonClick} className={styles.backButton}>
          <FontAwesomeIcon icon={faArrowLeft} />
        </button>
        {cardTitle && <div className={styles.cardTitle}>{cardTitle}</div>}
      </div>
      <div className={styles.contentWrapper}>
        <SideBar activeTab={activeTab} handleTabChange={handleTabChange} />
        <MainContent activeTab={activeTab} content={cardContent} />
      </div>
      <Footer />
    </div>
  );
};

export default SideBarPage;







import React from "react";
import styles from "./MainContent.module.css";
import Video from "./Video";

const MainContent = ({ activeTab, content }) => {
  const contentMap = {
    description: (
      <div>
        <h2>Description</h2>
        <p>{content.description}</p>
      </div>
    ),
    solutionFlow: (
      <div>
        <h2>Solution Flow</h2>
        <img src={content.solutionFlow} alt="Solution Flow" />
      </div>
    ),
    demo: (
      <div>
        <h2>Demo</h2>
        <Video src={content.demo} />
      </div>
    ),
    techArchitecture: (
      <div>
        <h2>Technical Architecture</h2>
        <img src={content.techArchitecture} alt="Technical Architecture" />
      </div>
    ),
    benefits: (
      <div>
        <h2>Benefits</h2>
        <p>{content.benefits}</p>
      </div>
    ),
  };

  return <div className={styles.mainContent}>{contentMap[activeTab]}</div>;
};

export default MainContent;
