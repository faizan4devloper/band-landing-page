// App.js
import React from "react";
import { BrowserRouter as Router, Routes, Route } from "react-router-dom";
import Header from "./components/Header/Header";
import MyCarousel from "./components//Carousel/MyCarousel";
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






// src/components/MainContent.js
import React from "react";
import styles from "./MainContent.module.css";
import SolutionImg from "./Icons/solutionFlowGraph.png";
import TechnicalArchitecture from "./Icons/technicalArchitecture.png";
import videoDemoUrl from "./Icons/Email-EAR.mp4";
import Video from "./Video";

const MainContent = ({ activeTab }) => {
  const contentMap = {
    description: (
      <div>
        <h2>Description</h2>
        <p>
          It is a long established fact that a reader will be distracted by the
          readable content of a page when looking at its layout. The point of
          using Lorem Ipsum is that it has a more-or-less normal distribution of
          letters, as opposed to using 'Content here, content here', making it
          look like readable English. Many desktop publishing packages and web
          page editors now use Lorem Ipsum as their default model text, and a
          search for 'lorem ipsum' will uncover many web sites still in their
          infancy. Various versions have evolved over the years, sometimes by
          accident, sometimes on purpose (injected humour and the like). <br />{" "}
          It is a long established fact that a reader will be distracted by the
          readable content of a page when looking at its layout. The point of
          using Lorem Ipsum is that it has a more-or-less normal distribution of
          letters, as opposed to using 'Content here, content here', making it
          look like readable English. Many desktop publishing packages and web
          page editors now use Lorem Ipsum as their default model text, and a
          search for 'lorem ipsum' will uncover many web sites still in their
          infancy. Various versions have evolved over the years, sometimes by
          accident, sometimes on purpose (injected humour and the like). <br />
          It is a long established fact that a reader will be distracted by the
          readable content of a page when looking at its layout. The point of
          using Lorem Ipsum is that it has a more-or-less normal distribution of
          letters, as opposed to using 'Content here, content here', making it
          look like readable English. Many desktop publishing packages and web
          page editors now use Lorem Ipsum as their default model text, and a
          search for 'lorem ipsum' will uncover many web sites still in their
          infancy. Various versions have evolved over the years, sometimes by
          accident, sometimes on purpose (injected humour and the like).
        </p>
      </div>
    ),
    solutionFlow: (
      <div>
        <h2>Solution Flow</h2>
        <img src={SolutionImg} alt="" />
      </div>
    ),
    demo: (
      <div>
        <Video src={videoDemoUrl} />
      </div>
    ),
    techArchitecture: (
      <div>
        <h2>Technical Architecture</h2>
        <img src={TechnicalArchitecture} alt="" />
      </div>
    ),
    benefits: (
      <div>
        <h2>Benefits</h2>
        <p>
          1. It is a long established fact that a reader will be distracted by
          the readable content of a page when looking at its layout. The point
          of using Lorem Ipsum is that it has a more-or-less normal distribution
          of letters, as opposed to using 'Content here, content here', making
          it look like readable English. Many desktop publishing packages and
          web page editors now use Lorem Ipsum as their default model text, and
          a search for 'lorem ipsum' will uncover many web sites still in their
          infancy. Various versions have evolved over the years, sometimes by
          accident, sometimes on purpose (injected humour and the like).
        </p>
        <p>
          2. It is a long established fact that a reader will be distracted by
          the readable content of a page when looking at its layout. The point
          of using Lorem Ipsum is that it has a more-or-less normal distribution
          of letters, as opposed to using 'Content here, content here', making
          it look like readable English. Many desktop publishing packages and
          web page editors now use Lorem Ipsum as their default model text, and
          a search for 'lorem ipsum' will uncover many web sites still in their
          infancy. Various versions have evolved over the years, sometimes by
          accident, sometimes on purpose (injected humour and the like).
        </p>
        <p>
          3. It is a long established fact that a reader will be distracted by
          the readable content of a page when looking at its layout. The point
          of using Lorem Ipsum is that it has a more-or-less normal distribution
          of letters, as opposed to using 'Content here, content here', making
          it look like readable English. Many desktop publishing packages and
          web page editors now use Lorem Ipsum as their default model text, and
          a search for 'lorem ipsum' will uncover many web sites still in their
          infancy. Various versions have evolved over the years, sometimes by
          accident, sometimes on purpose (injected humour and the like).
        </p>
        <p>
          4. It is a long established fact that a reader will be distracted by
          the readable content of a page when looking at its layout. The point
          of using Lorem Ipsum is that it has a more-or-less normal distribution
          of letters, as opposed to using 'Content here, content here', making
          it look like readable English. Many desktop publishing packages and
          web page editors now use Lorem Ipsum as their default model text, and
          a search for 'lorem ipsum' will uncover many web sites still in their
          infancy. Various versions have evolved over the years, sometimes by
          accident, sometimes on purpose (injected humour and the like).
        </p>
      </div>
    ),
  };

  return <div className={styles.mainContent}>{contentMap[activeTab]}</div>;
};

export default MainContent;


// src/components/MainContent.js
// import React from "react";
// import styles from "./MainContent.module.css";
// import SolutionImg1 from "./Icons/SolutionFlow.svg";
// import SolutionImg2 from "./Icons/Benefits.svg";
// import SolutionImg3 from "./Icons/Description.svg";
// import SolutionImg4 from "./Icons/ArchitectureFlow.svg";
// import SolutionImg5 from "./Icons/solutionFlowGraph.png";
// import videoDemoUrl1 from "./Icons/Email-EAR.mp4";
// import videoDemoUrl2 from "./Icons/SignatureExtraction.mp4";
// import videoDemoUrl3 from "./Icons/IntelligentAssist.mp4";
// import videoDemoUrl4 from "./Icons/CaseIntelligent.mp4";
// import videoDemoUrl5 from "./Icons/CodeGreat.mp4";
// import Video from "./Video";

// const MainContent = ({ activeTab }) => {
//   const contentMap = {
//     emailEAR: (
//       <div>
//         <h2>Email EAR</h2>
//         <Video src={videoDemoUrl1} />
//         <img src={SolutionImg1} alt="Solution Flow" />
//         <p>Description of Email EAR solution...</p>
//       </div>
//     ),
//     signatureExtraction: (
//       <div>
//         <h2>Signature Extraction & Verification</h2>
//         <Video src={videoDemoUrl2} />
//         <img src={SolutionImg2} alt="Solution Flow" />
//         <p>Description of Signature Extraction & Verification solution...</p>
//       </div>
//     ),
//     intelligentAssist: (
//       <div>
//         <h2>Intelligent Assist (Q & A)</h2>
//         <Video src={videoDemoUrl3} />
//         <img src={SolutionImg3} alt="Solution Flow" />
//         <p>Description of Intelligent Assist solution...</p>
//       </div>
//     ),
//     caseIntelligent: (
//       <div>
//         <h2>Case Intelligent</h2>
//         <Video src={videoDemoUrl4} />
//         <img src={SolutionImg4} alt="Solution Flow" />
//         <p>Description of Case Intelligent solution...</p>
//       </div>
//     ),
//     codeGreat: (
//       <div>
//         <h2>Code Great</h2>
//         <Video src={videoDemoUrl5} />
//         <img src={SolutionImg5} alt="Solution Flow" />
//         <p>Description of Code Great solution...</p>
//       </div>
//     ),
//   };

//   return <div className={styles.mainContent}>{contentMap[activeTab]}</div>;
// };

// export default MainContent;


/* eslint-disable jsx-a11y/alt-text */
import React from "react";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faAngleRight } from "@fortawesome/free-solid-svg-icons";
import styles from "./SideBar.module.css";

// Import individual images for each menu item
import descriptionImg from "./Icons/Description.svg";
import solutionFlowImg from "./Icons/SolutionFlow.svg";
import demoImg from "./Icons/Demo.svg";
import techArchitectureImg from "./Icons/ArchitectureFlow.svg";
import benefitsImg from "./Icons/Benefits.svg";

const SideBar = ({ activeTab, handleTabChange }) => {
  const menuItems = [
    { id: "description", label: "Description", img: descriptionImg },
    { id: "solutionFlow", label: "Solution Flow", img: solutionFlowImg },
    { id: "demo", label: "Demo", img: demoImg },
    {
      id: "techArchitecture",
      label: "Technical Architecture",
      img: techArchitectureImg,
    },
    { id: "benefits", label: "Benefits/Use Cases", img: benefitsImg },
  ];

  return (
    <nav className={styles.sideBar}>
      {menuItems.map((item) => (
        <button
          key={item.id}
          onClick={() => handleTabChange(item.id)}
          className={`${styles.menuItem} ${
            activeTab === item.id ? styles.active : ""
          }`}
        >
          <img
            src={item.img}
            className={`${styles.iconImg} ${styles.hoverEffect}`}
          />
          <span className={styles.label}>{item.label}</span>
          <FontAwesomeIcon icon={faAngleRight} className={styles.icon} />
        </button>
      ))}
    </nav>
  );
};

export default SideBar;



import React, { useState, useEffect } from "react";
import { useLocation } from "react-router-dom";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faArrowLeft } from "@fortawesome/free-solid-svg-icons";
import Header from "../Header/Header";
import Footer from "../Footer/Footer";
import SideBar from "./SideBar";
import MainContent from "./MainContent";
import styles from "./SideBarPage.module.css"; // Import the module.css file for styling

const SideBarPage = () => {
  const [activeTab, setActiveTab] = useState("description");
  const [cardTitle, setCardTitle] = useState("");
  const location = useLocation();

  useEffect(() => {
    // Extract card title from query parameter
    const params = new URLSearchParams(location.search);
    const title = params.get("title");
    if (title) {
      setCardTitle(title);
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
        <MainContent activeTab={activeTab} />
      </div>
      <Footer />
    </div>
  );
};

export default SideBarPage;

