// src/components/MainContent.js
import React from "react";
import styles from "./MainContent.module.css";
import SolutionImg1 from "./Icons/solutionFlowGraph1.png";
import SolutionImg2 from "./Icons/solutionFlowGraph2.png";
import SolutionImg3 from "./Icons/solutionFlowGraph3.png";
import SolutionImg4 from "./Icons/solutionFlowGraph4.png";
import SolutionImg5 from "./Icons/solutionFlowGraph5.png";
import videoDemoUrl1 from "./Icons/Email-EAR.mp4";
import videoDemoUrl2 from "./Icons/SignatureExtraction.mp4";
import videoDemoUrl3 from "./Icons/IntelligentAssist.mp4";
import videoDemoUrl4 from "./Icons/CaseIntelligent.mp4";
import videoDemoUrl5 from "./Icons/CodeGreat.mp4";
import Video from "./Video";

const MainContent = ({ activeTab }) => {
  const contentMap = {
    emailEAR: (
      <div>
        <h2>Email EAR</h2>
        <Video src={videoDemoUrl1} />
        <img src={SolutionImg1} alt="Solution Flow" />
        <p>Description of Email EAR solution...</p>
      </div>
    ),
    signatureExtraction: (
      <div>
        <h2>Signature Extraction & Verification</h2>
        <Video src={videoDemoUrl2} />
        <img src={SolutionImg2} alt="Solution Flow" />
        <p>Description of Signature Extraction & Verification solution...</p>
      </div>
    ),
    intelligentAssist: (
      <div>
        <h2>Intelligent Assist (Q & A)</h2>
        <Video src={videoDemoUrl3} />
        <img src={SolutionImg3} alt="Solution Flow" />
        <p>Description of Intelligent Assist solution...</p>
      </div>
    ),
    caseIntelligent: (
      <div>
        <h2>Case Intelligent</h2>
        <Video src={videoDemoUrl4} />
        <img src={SolutionImg4} alt="Solution Flow" />
        <p>Description of Case Intelligent solution...</p>
      </div>
    ),
    codeGreat: (
      <div>
        <h2>Code Great</h2>
        <Video src={videoDemoUrl5} />
        <img src={SolutionImg5} alt="Solution Flow" />
        <p>Description of Code Great solution...</p>
      </div>
    ),
  };

  return <div className={styles.mainContent}>{contentMap[activeTab]}</div>;
};

export default MainContent;




import React from "react";
import { BrowserRouter as Router, Routes, Route } from "react-router-dom";
import Header from "./components/Header/Header";
import MyCarousel from "./components//Carousel/MyCarousel";
import Cards from "./components/Cards/Cards";
import Footer from "./components/Footer/Footer";
import styles from "./App.module.css";
import SideBarPage from "./components/Sidebar/SideBarPage";
import MainContent from "./components/MainContent";
import { cardsData } from "./data";

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
          {cardsData.map((card, index) => (
            <Route
              key={index}
              path={`/${card.title.toLowerCase().replace(/ /g, "-")}`}
              element={<MainContent activeTab={card.title.toLowerCase().replace(/ /g, "")} />}
            />
          ))}
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
