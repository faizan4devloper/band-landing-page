import React, { useState, useEffect } from "react";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faBars, faTimes, faChevronDown, faChevronUp } from "@fortawesome/free-solid-svg-icons";
import "./Chat.css";

export function Chat({ onQuestionClick, onSchoolClick }) {
    const [data, setData] = useState(null);
    const [showQuestions, setShowQuestions] = useState(false);
    const [selectedQuestion, setSelectedQuestion] = useState(null);
    const [expandedQuestions, setExpandedQuestions] = useState({});
    const [expandedDescriptions, setExpandedDescriptions] = useState({});
    const [recent, setRecent] = useState([]);
    const [showRecent, setShowRecent] = useState(false);
    
    useEffect(() => {
        fetch('/data.json')
            .then(response => response.json())
            .then(data => setData(data))
            .catch(error => console.error('Error fetching data:', error));
    }, []);

    if (!data) {
        return <div>Loading...</div>;
    }

    const handleQuestionClick = (question) => {
        setShowQuestions(false);
        setSelectedQuestion(question);
        onQuestionClick(question);
        setShowRecent(false);
    };

    const handleSchoolClick = (school) => {
        onSchoolClick(school);
    };

    const toggleQuestions = () => {
        setShowQuestions(!showQuestions);
    };

    const toggleExpand = (question) => {
        setExpandedQuestions((prevExpanded) => ({
            ...prevExpanded,
            [question]: !prevExpanded[question],
        }));
    };

    const toggleExpandDescription = (question, index) => {
        setExpandedDescriptions((prevExpandedDescriptions) => ({
            ...prevExpandedDescriptions,
            [`${question}-${index}`]: !prevExpandedDescriptions[`${question}-${index}`]
        }));
        if (!showRecent) {
            updateRecent(question, index);
            setShowRecent(true);
        }
    };

    const updateRecent = (question, index) => {
        setRecent((prevRecent) => [
            ...prevRecent,
            {
                question,
                description: data.questionDescriptions[question],
                selectedDescription: data.questionDescriptions[question][index]
            }
        ]);
    };

    return (
        <div className="chat-container">
            <h1>Frequently Inquired Topics</h1>
            <div className="hamburger-menu" onClick={toggleQuestions}>
                <FontAwesomeIcon icon={showQuestions ? faTimes : faBars} />
            </div>
            <div className={`most-asked-questions ${showQuestions ? 'show' : ''}`}>
                <h4>Frequently Inquired Topics</h4>
                <ul>
                    {data.mostAskedQuestions.map((question, index) => (
                        <li key={index} onClick={() => handleQuestionClick(question)}>
                            {question}
                        </li>
                    ))}
                </ul>
            </div>
            <div className="selected-questions" style={{ display: showRecent ? 'none' : 'block' }}>
                {selectedQuestion && (
                    <div
                        key={selectedQuestion}
                        className={`selected-question ${expandedQuestions[selectedQuestion] ? 'expanded' : ''}`}
                        aria-expanded={expandedQuestions[selectedQuestion]}
                    >
                        <div className="question-header">
                            <h4>{selectedQuestion}</h4>
                            <FontAwesomeIcon
                                icon={expandedQuestions[selectedQuestion] ? faChevronUp : faChevronDown}
                                className="toggle-icon"
                                onClick={() => toggleExpand(selectedQuestion)}
                            />
                        </div>
                        {expandedQuestions[selectedQuestion] && (
                            <div className="descriptions active">
                                {data.questionDescriptions[selectedQuestion].map((desc, idx) => (
                                    <div
                                        key={idx}
                                        className={`description ${expandedDescriptions[`${selectedQuestion}-${idx}`] ? 'expanded' : ''}`}
                                        onClick={() => toggleExpandDescription(selectedQuestion, idx)}
                                    >
                                        <p>{desc}</p>
                                    </div>
                                ))}
                            </div>
                        )}
                    </div>
                )}
            </div>
            <div className={`recent ${showRecent ? 'show' : ''}`}>
                <h2>Recent Topics</h2>
                <ul>
                    {recent.map((item, index) => (
                        <li key={index}>
                            <h4>{item.question}</h4>
                            <ul>
                                <li>{item.selectedDescription}</li>
                                {data.schoolNames.map((school, schoolIdx) => (
                                    <li key={schoolIdx} onClick={() => handleSchoolClick(school)}>
                                        {school}
                                    </li>
                                ))}
                            </ul>
                        </li>
                    ))}
                </ul>
            </div>
        </div>
    );
}





.chat-container {
    background-color: rgba(0, 0, 0, 0.5);
    margin: 80px 0 0 10px;
    border-radius: 10px;
    width: 450px;
    display: flex;
    flex-direction: column;
    height: 500px;
    position: relative;
    overflow: hidden;
    box-shadow: 0 10px 20px rgba(0, 0, 0, 0.3);
}

.chat-container h1 {
    text-align: center;
    margin-top: 14px;
    font-size: 1.5rem;
    line-height: 1.2;
    font-weight: 600;
    background: linear-gradient(90deg, #7abef7 -19.51%, #4080f5 36.51%, #572ac2 100%);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
    text-fill-color: transparent;
    text-transform: inherit !important;
}

.hamburger-menu {
    position: absolute;
    top: 10px;
    right: 10px;
    font-size: 24px;
    color: #fff;
    cursor: pointer;
    z-index: 10;
}

.most-asked-questions {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: linear-gradient(90deg, #6f36cd 0%, #1f77f6 100%);
    display: flex;
    border-radius: 10px;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    z-index: 5;
    color: white;
    transform: translateY(-100%);
    transition: transform 0.3s ease-in-out;
}

.most-asked-questions.show {
    transform: translateY(0);
}

.most-asked-questions h4 {
    margin-bottom: 20px;
    opacity: 0;
    animation: fadeIn 0.5s 0.2s forwards;
}

.most-asked-questions ul {
    list-style: none;
    padding: 0;
    opacity: 0;
    animation: fadeIn 0.5s 0.4s forwards;
}

.most-asked-questions li {
    padding: 10px 20px;
    font-size: 14px;
    font-weight: 500;
    margin: 5px 0;
    background-color: #fff;
    color: #000;
    border-radius: 4px;
    cursor: pointer;
    box-shadow: 0 10px 8px rgba(0, 0, 0, 0.2);
    transition: background-color 0.3s ease-in-out, transform 0.3s ease-in-out;
}

.most-asked-questions li:hover {
    background-color: rgba(0, 0, 0, 0.5);
    color: #fff;
    transform: scale(1.05);
}

.selected-questions {
    margin-top: 30px;
    overflow-y: auto;
    padding: 10px;
    flex-grow: 1;
}

.selected-questions::-webkit-scrollbar {
    width: 8px;
}

.selected-questions::-webkit-scrollbar-thumb {
    background-color: rgba(111, 54, 205, 0.8);
    border-radius: 10px;
}

.selected-questions::-webkit-scrollbar-thumb:hover {
    background-color: rgba(111, 54, 205, 1);
}

.selected-questions::-webkit-scrollbar-track {
    background-color: rgba(255, 255, 255, 0.1);
    border-radius: 10px;
}

.selected-question {
    background-color: rgba(255, 255, 255, 0.1);
    padding: 10px;
    border-radius: 4px;
    margin-bottom: 10px;
    box-shadow: 0 10px 8px rgba(0, 0, 0, 0.2);
    color: #fff;
    cursor: pointer;
    transition: background-color 0.3s ease-in-out, transform 0.3s ease-in-out;
}

.selected-question h4 {
    margin: 0;
}

.selected-question .question-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.selected-question .toggle-icon {
    font-size: 18px;
}

.selected-question p {
    margin: 10px 0 0;
    display: none;
    animation: fadeIn 0.3s ease-in-out forwards;
}

.selected-question.expanded p {
    font-size: 14px;
    display: block;
    transition: 0.1s ease-in;
}

.selected-question.expanded p:hover {
    color: #000;
}

.description {
    background-color: rgba(255, 255, 255, 0.1);
    display: none;
    padding: 8px;
    border-radius: 4px;
    margin: 8px 0;
    cursor: pointer;
    transition: background-color 0.3s ease-in-out, transform 0.3s ease-in-out;
}

.description:hover {
    background-color: rgba(255, 255, 255, 0.2);
}

.selected-question.expanded .description {
    display: block;
}

.description.expanded ul {
    display: block;
}

.description ul {
    list-style: none;
    padding: 0;
    margin: 10px 0 0;
    display: none;
    animation: fadeIn 0.3s ease-in-out forwards;
}

.description ul li {
    background-color: #e0e0e0;
    text-align: center;
    padding: 5px;
    color: #000;
    border-radius: 4px;
    margin: 5px 0;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    transition: background-color 0.3s ease-in-out, transform 0.3s ease-in-out;
}

.description ul li:hover {
    background: #c0c0ef;
}

.recent {
    margin-top: 30px;
    padding: 20px;
    background: rgba(255, 255, 255, 0.1);
    border: 1px solid rgba(255, 255, 255, 0.2);
    border-radius: 10px;
    box-shadow: 0 10px 20px rgba(0, 0, 0, 0.2);
    max-height: 0;
    overflow-y: auto;
    transition: max-height 0.3s ease-in-out, opacity 0.3s ease-in-out;
    opacity: 0;
}

.recent.show {
    max-height: 400px; /* Adjust this value as needed */
    opacity: 1;
}

.recent h2 {
    margin: 0;
    font-size: 18px;
    text-align: center;
    line-height: 1.2;
    font-weight: 600;
    background: linear-gradient(90deg, #7abef7 -19.51%, #4080f5 36.51%, #572ac2 100%);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
    text-fill-color: transparent;
    text-transform: inherit !important;
    border-bottom: 2px solid #fff;
    padding-bottom: 10px;
}

.recent ul {
    list-style: none;
    padding: 0;
}

.recent li {
    margin-bottom: 15px;
}

.recent li h4 {
    margin: 0;
    font-size: 16px;
    color: #fff;
}

.recent li ul {
    list-style: disc inside;
    margin-top: 10px;
    padding-left: 20px;
}

.recent li ul li {
    margin-bottom: 5px;
    color: #ddd;
}

@keyframes fadeIn {
    from {
        opacity: 0;
    }
    to {
        opacity: 1;
    }
}







followed by Admission Trends - Easy of Admission - Last 5 year data:-
Year
	
# of Applications Received
	
# of Applications Accepted


2020-2021
	
800
	
250


2021-2022
	
1000
	
300


2022-2023
	
1200
	
300


2023-2024
	
1500
	
350


2204-2025
	
2000 (Forecasted)
	
380 (Forecasted)
the amount of application accepted / # of applications applied
Success of Appeal Process-
 
Year
	
# of Appeals Received
	
# of Applications Accepted


2020-2021
	
20
	
4


2021-2022
	
30
	
6


2022-2023
	
35
	
8


2023-2024
	
40
	
10


2204-2025
	
43 (Forecasted)
	
12 (Forecasted)
 
import React, { useState } from "react";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faAngleRight, faAngleDown } from "@fortawesome/free-solid-svg-icons";
import "./Insights.css";
import ParentReview from "./ParentReview";

export function Insights({ selectedSchool }){
  const [popupContent, setPopupContent] = useState(null);
  const [expandedSection, setExpandedSection] = useState(null);

  const handleExpand = (section) => {
    if (expandedSection === section) {
      setExpandedSection(null);
    } else {
      setExpandedSection(section);
    }
  };

  const handlePopup = () => {
    setPopupContent("parentReviews");
  };

  const closePopup = () => {
    setPopupContent(null);
  };

  return (
    <div className="insights-container">
      <h3>Insights : {selectedSchool}</h3>
      {selectedSchool && (
        <div>
          <div className="section">
            <h4 onClick={() => handleExpand("parentReviews")}>
              <span className="expand-icon">
                {expandedSection === "parentReviews" ? (
                  <FontAwesomeIcon icon={faAngleRight} />
                ) : (
                  <FontAwesomeIcon icon={faAngleDown} />
                )}
              </span>
              Parent Reviews
            </h4>
            {expandedSection === "parentReviews" && (
              <div className="section-content">
                <p>Overall Rating - one liner</p>
                <p onClick={handlePopup}>Review Summary</p>
              </div>
            )}
          </div>
          <div className="section">
            <h4 onClick={() => handleExpand("ofstedRating")}>
              <span className="expand-icon">
                {expandedSection === "ofstedRating" ? (
                  <FontAwesomeIcon icon={faAngleRight} />
                ) : (
                  <FontAwesomeIcon icon={faAngleDown} />
                )}
              </span>
              Ofsted Rating
            </h4>
            {expandedSection === "ofstedRating" && (
              <div className="section-content">
                <textarea
                  placeholder="Enter Ofsted Rating here..."
                  rows={5}
                ></textarea>
              </div>
            )}
          </div>
          <div className="section">
            <h4 onClick={() => handleExpand("admissionTrend")}>
              <span className="expand-icon">
                {expandedSection === "admissionTrend" ? (
                  <FontAwesomeIcon icon={faAngleRight} />
                ) : (
                  <FontAwesomeIcon icon={faAngleDown} />
                )}
              </span>
              Admission Trend
            </h4>
            {expandedSection === "admissionTrend" && (
              <div className="section-content">
                <p>Admissions: Enter admissions details here...</p>
                <p>Appeals: Enter appeals details here...</p>
              </div>
            )}
          </div>
          <div className="section">
            <h4 onClick={() => handleExpand("termPlan")}>
              <span className="expand-icon">
                {expandedSection === "termPlan" ? (
                  <FontAwesomeIcon icon={faAngleRight} />
                ) : (
                  <FontAwesomeIcon icon={faAngleDown} />
                )}
              </span>
              Term Plan
            </h4>
            {expandedSection === "termPlan" && (
              <div className="section-content">
                <p onClick={() => handlePopup("PDF link")}>
                  PDF link (pop up view)
                </p>
              </div>
            )}
          </div>
        </div>
      )}
      {popupContent === "parentReviews" && (
        <ParentReview closePopup={closePopup} />
      )}
    </div>
  );
}


import React, { useState } from "react";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faAngleRight, faAngleDown } from "@fortawesome/free-solid-svg-icons";
import { LineChart, Line, XAxis, YAxis, CartesianGrid, Tooltip, Legend, ResponsiveContainer } from 'recharts';
import "./Insights.css";
import ParentReview from "./ParentReview";

export function Insights({ selectedSchool }) {
  const [popupContent, setPopupContent] = useState(null);
  const [expandedSection, setExpandedSection] = useState(null);

  const handleExpand = (section) => {
    if (expandedSection === section) {
      setExpandedSection(null);
    } else {
      setExpandedSection(section);
    }
  };

  const handlePopup = () => {
    setPopupContent("parentReviews");
  };

  const closePopup = () => {
    setPopupContent(null);
  };

  const admissionsData = [
    { year: '2020-2021', applicationsReceived: 800, applicationsAccepted: 250 },
    { year: '2021-2022', applicationsReceived: 1000, applicationsAccepted: 300 },
    { year: '2022-2023', applicationsReceived: 1200, applicationsAccepted: 300 },
    { year: '2023-2024', applicationsReceived: 1500, applicationsAccepted: 350 },
    { year: '2024-2025', applicationsReceived: 2000, applicationsAccepted: 380 },
  ];

  const appealsData = [
    { year: '2020-2021', appealsReceived: 20, appealsAccepted: 4 },
    { year: '2021-2022', appealsReceived: 30, appealsAccepted: 6 },
    { year: '2022-2023', appealsReceived: 35, appealsAccepted: 8 },
    { year: '2023-2024', appealsReceived: 40, appealsAccepted: 10 },
    { year: '2024-2025', appealsReceived: 43, appealsAccepted: 12 },
  ];

  return (
    <div className="insights-container">
      <h3>Insights : {selectedSchool}</h3>
      {selectedSchool && (
        <div>
          <div className="section">
            <h4 onClick={() => handleExpand("parentReviews")}>
              <span className="expand-icon">
                {expandedSection === "parentReviews" ? (
                  <FontAwesomeIcon icon={faAngleRight} />
                ) : (
                  <FontAwesomeIcon icon={faAngleDown} />
                )}
              </span>
              Parent Reviews
            </h4>
            {expandedSection === "parentReviews" && (
              <div className="section-content">
                <p>Overall Rating - one liner</p>
                <p onClick={handlePopup}>Review Summary</p>
              </div>
            )}
          </div>
          <div className="section">
            <h4 onClick={() => handleExpand("ofstedRating")}>
              <span className="expand-icon">
                {expandedSection === "ofstedRating" ? (
                  <FontAwesomeIcon icon={faAngleRight} />
                ) : (
                  <FontAwesomeIcon icon={faAngleDown} />
                )}
              </span>
              Ofsted Rating
            </h4>
            {expandedSection === "ofstedRating" && (
              <div className="section-content">
                <textarea
                  placeholder="Enter Ofsted Rating here..."
                  rows={5}
                ></textarea>
              </div>
            )}
          </div>
          <div className="section">
            <h4 onClick={() => handleExpand("admissionTrend")}>
              <span className="expand-icon">
                {expandedSection === "admissionTrend" ? (
                  <FontAwesomeIcon icon={faAngleRight} />
                ) : (
                  <FontAwesomeIcon icon={faAngleDown} />
                )}
              </span>
              Admission Trend
            </h4>
            {expandedSection === "admissionTrend" && (
              <div className="section-content">
                <p>Admissions: Enter admissions details here...</p>
                <ResponsiveContainer width="100%" height={300}>
                  <LineChart data={admissionsData}>
                    <CartesianGrid strokeDasharray="3 3" />
                    <XAxis dataKey="year" />
                    <YAxis />
                    <Tooltip />
                    <Legend />
                    <Line type="monotone" dataKey="applicationsReceived" stroke="#8884d8" />
                    <Line type="monotone" dataKey="applicationsAccepted" stroke="#82ca9d" />
                  </LineChart>
                </ResponsiveContainer>
                <p>Appeals: Enter appeals details here...</p>
                <ResponsiveContainer width="100%" height={300}>
                  <LineChart data={appealsData}>
                    <CartesianGrid strokeDasharray="3 3" />
                    <XAxis dataKey="year" />
                    <YAxis />
                    <Tooltip />
                    <Legend />
                    <Line type="monotone" dataKey="appealsReceived" stroke="#8884d8" />
                    <Line type="monotone" dataKey="appealsAccepted" stroke="#82ca9d" />
                  </LineChart>
                </ResponsiveContainer>
              </div>
            )}
          </div>
          <div className="section">
            <h4 onClick={() => handleExpand("termPlan")}>
              <span className="expand-icon">
                {expandedSection === "termPlan" ? (
                  <FontAwesomeIcon icon={faAngleRight} />
                ) : (
                  <FontAwesomeIcon icon={faAngleDown} />
                )}
              </span>
              Term Plan
            </h4>
            {expandedSection === "termPlan" && (
              <div className="section-content">
                <p onClick={() => handlePopup("PDF link")}>
                  PDF link (pop up view)
                </p>
              </div>
            )}
          </div>
        </div>
      )}
      {popupContent === "parentReviews" && (
        <ParentReview closePopup={closePopup} />
      )}
    </div>
  );
}



.insights-container {
  padding: 20px;
}

.section {
  margin-bottom: 20px;
}

.section h4 {
  cursor: pointer;
  display: flex;
  align-items: center;
}

.expand-icon {
  margin-right: 10px;
}

.section-content {
  padding: 10px;
  background-color: #f9f9f9;
  border-radius: 4px;
}

.line-chart-container {
  margin-top: 20px;
}




import React, { useState } from "react";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faAngleRight, faAngleDown } from "@fortawesome/free-solid-svg-icons";
import "./Insights.css";
import ParentReview from "./ParentReview";
import AdmissionChart from "./AdmissionChart";
import AppealChart from "./AppealChart";

export function Insights({ selectedSchool }) {
  const [popupContent, setPopupContent] = useState(null);
  const [expandedSection, setExpandedSection] = useState(null);
  const [admissionsChartVisible, setAdmissionsChartVisible] = useState(false);
  const [appealsChartVisible, setAppealsChartVisible] = useState(false);

  const handleExpand = (section) => {
    if (section === "admissionTrend") {
      setAdmissionsChartVisible(true);
      setAppealsChartVisible(false);
    } else if (section === "appealTrend") {
      setAppealsChartVisible(true);
      setAdmissionsChartVisible(false);
    }
    setExpandedSection(section);
  };

  const handlePopup = () => {
    setPopupContent("parentReviews");
  };

  const closePopup = () => {
    setPopupContent(null);
  };

  return (
    <div className="insights-container">
      <h3>Insights : {selectedSchool}</h3>
      {selectedSchool && (
        <div>
          <div className="section">
            <h4 onClick={() => handleExpand("parentReviews")}>
              <span className="expand-icon">
                {expandedSection === "parentReviews" ? (
                  <FontAwesomeIcon icon={faAngleRight} />
                ) : (
                  <FontAwesomeIcon icon={faAngleDown} />
                )}
              </span>
              Parent Reviews
            </h4>
            {expandedSection === "parentReviews" && (
              <div className="section-content">
                <p>Overall Rating - one liner</p>
                <p onClick={handlePopup}>Review Summary</p>
              </div>
            )}
          </div>
          <div className="section">
            <h4 onClick={() => handleExpand("admissionTrend")}>
              <span className="expand-icon">
                {expandedSection === "admissionTrend" ? (
                  <FontAwesomeIcon icon={faAngleRight} />
                ) : (
                  <FontAwesomeIcon icon={faAngleDown} />
                )}
              </span>
              Admission Trend
            </h4>
            {expandedSection === "admissionTrend" && (
              <div className="section-content">
                <AdmissionChart />
              </div>
            )}
          </div>
          <div className="section">
            <h4 onClick={() => handleExpand("appealTrend")}>
              <span className="expand-icon">
                {expandedSection === "appealTrend" ? (
                  <FontAwesomeIcon icon={faAngleRight} />
                ) : (
                  <FontAwesomeIcon icon={faAngleDown} />
                )}
              </span>
              Appeal Trend
            </h4>
            {expandedSection === "appealTrend" && (
              <div className="section-content">
                <AppealChart />
              </div>
            )}
          </div>
          <div className="section">
            <h4 onClick={() => handleExpand("termPlan")}>
              <span className="expand-icon">
                {expandedSection === "termPlan" ? (
                  <FontAwesomeIcon icon={faAngleRight} />
                ) : (
                  <FontAwesomeIcon icon={faAngleDown} />
                )}
              </span>
              Term Plan
            </h4>
            {expandedSection === "termPlan" && (
              <div className="section-content">
                <p onClick={() => handlePopup("PDF link")}>
                  PDF link (pop up view)
                </p>
              </div>
            )}
          </div>
        </div>
      )}
      {popupContent === "parentReviews" && (
        <ParentReview closePopup={closePopup} />
      )}
    </div>
  );
}
