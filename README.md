
// data.js
import image1 from "./components/Cards/card1.jpg";
import image2 from "./components/Cards/card2.jpg";
import image3 from "./components/Cards/card3.jpg";
import image4 from "./components/Cards/card4.jpg";
import image5 from "./components/Cards/card5.jpg";

export const cardsData = [
  {
    imageUrl: image1,
    title: "Email EAR",
    description: "It is a long established fact that a reader will be distracted by the readable content of a page when looking at its layout.",
    content: {
      description: "Detailed description for Email EAR...",
      solutionFlow: "Solution flow for Email EAR...",
      demo: "Demo for Email EAR...",
      techArchitecture: "Technical architecture for Email EAR...",
      benefits: "Benefits of Email EAR...",
    },
  },
  {
    imageUrl: image2,
    title: "Signature extraction & verification",
    description: "It is a long established fact that a reader will be distracted by the readable content of a page when looking at its layout.",
    content: {
      description: "Detailed description for Signature extraction & verification...",
      solutionFlow: "Solution flow for Signature extraction & verification...",
      demo: "Demo for Signature extraction & verification...",
      techArchitecture: "Technical architecture for Signature extraction & verification...",
      benefits: "Benefits of Signature extraction & verification...",
    },
  },
  {
    imageUrl: image3,
    title: "Intelligent Assist (Q & A)",
    description: "It is a long established fact that a reader will be distracted by the readable content of a page when looking at its layout.",
    content: {
      description: "Detailed description for Intelligent Assist...",
      solutionFlow: "Solution flow for Intelligent Assist...",
      demo: "Demo for Intelligent Assist...",
      techArchitecture: "Technical architecture for Intelligent Assist...",
      benefits: "Benefits of Intelligent Assist...",
    },
  },
  {
    imageUrl: image4,
    title: "Case Intelligent",
    description: "It is a long established fact that a reader will be distracted by the readable content of a page when looking at its layout.",
    content: {
      description: "Detailed description for Case Intelligent...",
      solutionFlow: "Solution flow for Case Intelligent...",
      demo: "Demo for Case Intelligent...",
      techArchitecture: "Technical architecture for Case Intelligent...",
      benefits: "Benefits of Case Intelligent...",
    },
  },
  {
    imageUrl: image5,
    title: "Code Great",
    description: "It is a long established fact that a reader will be distracted by the readable content of a page when looking at its layout.",
    content: {
      description: "Detailed description for Code Great...",
      solutionFlow: "Solution flow for Code Great...",
      demo: "Demo for Code Great...",
      techArchitecture: "Technical architecture for Code Great...",
      benefits: "Benefits of Code Great...",
    },
  },
];





// components/Cards/Cards.js
import React, { useState } from "react";
import { Link } from "react-router-dom";
import styles from "./Cards.module.css";

const Card = ({ imageUrl, title, description, content, isBig, toggleSize }) => {
  const [isHovered, setIsHovered] = useState(false);

  return (
    <div className={styles.cardsContainer}>
      <div
        className={`${styles.card} ${isBig ? styles.big : ""}`}
        style={{ backgroundImage: `url(${imageUrl})` }}
        onClick={toggleSize}
      >
        {!isBig && <div className={styles.cardTitle}>{title}</div>}
        {isBig && (
          <div className={styles.cardContent}>
            <h2>{title}</h2>
            <p>{description}</p>
            <Link
              to={{
                pathname: "/dashboard",
                search: `?title=${encodeURIComponent(title)}&content=${encodeURIComponent(JSON.stringify(content))}`,
              }}
              className={styles.readMoreLink}
            >
              <span
                className={`${styles.arrow} ${styles.rightArrow} ${
                  isHovered ? styles.hovered : ""
                }`}
                onMouseEnter={() => setIsHovered(true)}
                onMouseLeave={() => setIsHovered(false)}
              >
                <span
                  style={{
                    fontSize: isHovered ? "0.8em" : "1em",
                  }}
                >
                  {isHovered && "Read More "}
                </span>
                <span
                  style={{
                    marginLeft: isHovered ? "5px" : "0",
                    fontSize: isHovered ? "1.3em" : "1em",
                  }}
                >
                  &#129106;
                </span>
              </span>
            </Link>
          </div>
        )}
      </div>
    </div>
  );
};

export default Card;





// SideBarPage.js
import React, { useState, useEffect } from "react";
import { useLocation } from "react-router-dom";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faArrowLeft } from "@fortawesome/free-solid-svg-icons";
import Header from "../Header/Header";
import Footer from "../Footer/Footer";
import SideBar from "./SideBar";
import MainContent from "./MainContent";
import styles from "./SideBarPage.module.css";

const SideBarPage = () => {
  const [activeTab, setActiveTab] = useState("description");
  const [cardTitle, setCardTitle] = useState("");
  const [cardContent, setCardContent] = useState({});
  const location = useLocation();

  useEffect(() => {
    const params = new URLSearchParams(location.search);
    const title = params.get("title");
    const content = JSON.parse(params.get("content"));
    if (title) {
      setCardTitle(title);
      setCardContent(content);
    }
  }, [location.search]);

  const handleTabChange = (tabId) => {
    setActiveTab(tabId);
  };

  const handleBackButtonClick = () => {
    window.history.back();
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
        <MainContent activeTab={activeTab} cardContent={cardContent} />
      </div>
      <Footer />
    </div>
  );
};

export default SideBarPage;






// MainContent.js
import React from "react";
import styles from "./MainContent.module.css";
import SolutionImg from "./Icons/solutionFlowGraph.png";
import TechnicalArchitecture from "./Icons/technicalArchitecture.png";
import videoDemoUrl from "./Icons/Email-EAR.mp4";
import Video from "./Video";

const MainContent = ({ activeTab, cardContent }) => {
  const contentMap = {
    description: (
      <div>
        <h2>Description</h2>
        <p>{cardContent.description}</p>
      </div>
    ),
    solutionFlow: (
      <div>
        <h2>Solution Flow</h2>
        <p>{cardContent.solutionFlow}</p>
      </div>
    ),
    demo: (
      <div>
        <Video src={videoDemoUrl} />
        <p>{cardContent.demo}</p>
      </div>
    ),
    techArchitecture: (
      <div>
        <h2>Technical Architecture</h2>
        <p>{cardContent.techArchitecture}</p>
      </div>
    ),
    benefits: (
      <div>
        <h2>Benefits</h2>
        <p>{cardContent.benefits}</p>
      </div>
    ),
  };

  return <div className={styles.mainContent}>{contentMap[activeTab]}</div>;
};

export default MainContent;







// components/Cards/Card.js
import React, { useState } from "react";
import { Link } from "react-router-dom";
import styles from "./Card.module.css";

const Card = ({ imageUrl, title, description, content, isBig, toggleSize }) => {
  const [isHovered, setIsHovered] = useState(false);

  return (
    <div className={styles.cardsContainer}>
      <div
        className={`${styles.card} ${isBig ? styles.big : ""}`}
        style={{ backgroundImage: `url(${imageUrl})` }}
        onClick={toggleSize}
      >
        {!isBig && <div className={styles.cardTitle}>{title}</div>}
        {isBig && (
          <div className={styles.cardContent}>
            <h2>{title}</h2>
            <p>{description}</p>
            <Link
              to={{
                pathname: "/dashboard",
                search: `?title=${encodeURIComponent(title)}&content=${encodeURIComponent(JSON.stringify(content))}`,
              }}
              className={styles.readMoreLink}
            >
              <span
                className={`${styles.arrow} ${styles.rightArrow} ${
                  isHovered ? styles.hovered : ""
                }`}
                onMouseEnter={() => setIsHovered(true)}
                onMouseLeave={() => setIsHovered(false)}
              >
                <span
                  style={{
                    fontSize: isHovered ? "0.8em" : "1em",
                  }}
                >
                  {isHovered && "Read More "}
                </span>
                <span
                  style={{
                    marginLeft: isHovered ? "5px" : "0",
                    fontSize: isHovered ? "1.3em" : "1em",
                  }}
                >
                  &#129106;
                </span>
              </span>
            </Link>
          </div>
        )}
      </div>
    </div>
  );
};

export default Card;






import React from "react";
import { BrowserRouter as Router, Route, Switch } from "react-router-dom";
import Home from "./Home";
import SideBarPage from "./components/SideBarPage/SideBarPage";
import { cardsData } from "./data";
import Header from "./components/Header/Header";
import Footer from "./components/Footer/Footer";
import styles from "./App.module.css";

const App = () => {
  return (
    <Router>
      <div className={styles.app}>
        <Header />
        <Switch>
          <Route exact path="/">
            <Home cardsData={cardsData} />
          </Route>
          <Route path="/dashboard">
            <SideBarPage />
          </Route>
        </Switch>
        <Footer />
      </div>
    </Router>
  );
};

export default App;







// Home.js
import React from "react";
import Card from "./components/Cards/Card";
import styles from "./Home.module.css";

const Home = ({ cardsData }) => {
  return (
    <div className={styles.cardsContainer}>
      {cardsData.map((card, index) => (
        <Card
          key={index}
          imageUrl={card.imageUrl}
          title={card.title}
          description={card.description}
          content={card.content} // Pass the content for each card
        />
      ))}
    </div>
  );
};

export default Home;








import React from "react";
import { BrowserRouter as Router, Route, Switch } from "react-router-dom";
import Home from "./Home";
import SideBarPage from "./components/SideBarPage/SideBarPage";
import { cardsData } from "./data";
import Header from "./components/Header/Header";
import Footer from "./components/Footer/Footer";
import styles from "./App.module.css";

const App = () => {
  return (
    <Router>
      <div className={styles.app}>
        <Header />
        <Switch>
          <Route exact path="/">
            <Home cardsData={cardsData} />
          </Route>
          <Route path="/dashboard">
            <SideBarPage />
          </Route>
        </Switch>
        <Footer />
      </div>
    </Router>
  );
};

export default App;
