// data.js
export const cardsData = [
  {
    id: "solution1",
    imageUrl: "path/to/image1.jpg",
    title: "Solution 1",
    description: "Description for Solution 1",
  },
  {
    id: "solution2",
    imageUrl: "path/to/image2.jpg",
    title: "Solution 2",
    description: "Description for Solution 2",
  },
  // Add more solutions as needed
];





// App.js
import React from "react";
import { BrowserRouter as Router, Routes, Route } from "react-router-dom";
import Header from "./components/Header/Header";
import MyCarousel from "./components/Carousel/MyCarousel";
import Cards from "./components/Cards/Cards";
import Footer from "./components/Footer/Footer";
import styles from "./App.module.css";
import SideBarPage from "./components/Sidebar/SideBarPage";
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
          <Route path="/dashboard/:solutionId" element={<SideBarPage />} />
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





// components/Cards/Cards.js
import React, { useState } from "react";
import { Link } from "react-router-dom";
import styles from "./Cards.module.css";

const Card = ({ imageUrl, title, description, isBig, toggleSize }) => {
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
                pathname: `/dashboard/${title}`,
                search: `?title=${encodeURIComponent(title)}`,
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






// components/Sidebar/MainContent.js
import React from "react";
import { useParams } from "react-router-dom";
import styles from "./MainContent.module.css";
import SolutionImg from "./Icons/solutionFlowGraph.png";
import TechnicalArchitecture from "./Icons/technicalArchitecture.png";
import videoDemoUrl from "./Icons/Email-EAR.mp4";
import Video from "./Video";

const MainContent = ({ activeTab }) => {
  const { solutionId } = useParams();

  const solutionContents = {
    solution1: {
      description: (
        <div>
          <h2>Description for Solution 1</h2>
          <p>Details about Solution 1...</p>
        </div>
      ),
      solutionFlow: (
        <div>
          <h2>Solution Flow for Solution 1</h2>
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
          <h2>Technical Architecture for Solution 1</h2>
          <img src={TechnicalArchitecture} alt="" />
        </div>
      ),
      benefits: (
        <div>
          <h2>Benefits for Solution 1</h2>
          <p>Benefits and use cases of Solution 1...</p>
        </div>
      ),
    },
    solution2: {
      description: (
        <div>
          <h2>Description for Solution 2</h2>
          <p>Details about Solution 2...</p>
        </div>
      ),
      solutionFlow: (
        <div>
          <h2>Solution Flow for Solution 2</h2>
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
          <h2>Technical Architecture for Solution 2</h2>
          <img src={TechnicalArchitecture} alt="" />
        </div>
      ),
      benefits: (
        <div>
          <h2>Benefits for Solution 2</h2>
          <p>Benefits and use cases of Solution 2...</p>
        </div>
      ),
    },
    // Add more solutions as needed
  };

  const contentMap = solutionContents[solutionId] || {};

  return <div className={styles.mainContent}>{contentMap[activeTab]}</div>;
};

export default MainContent;
