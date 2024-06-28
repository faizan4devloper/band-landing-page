import React, { useState, useEffect, useRef } from "react";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faBars, faTimes, faChevronDown, faChevronUp,faPaperPlane, faUser, faRobot  } from "@fortawesome/free-solid-svg-icons";
import axios from "axios";
import "./Topics.css";

export function Topics({ onQuestionClick, onSchoolClick }) {
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
                <h2>Conversation History</h2>
                <ul>
                    {recent.map((item, index) => (
                        <li key={index}>
                            <h4>{item.question}</h4>
                            <ul>
                                <li>{item.selectedDescription}</li>
                                <h4>Schools</h4>
                                <div className="school-btns">
                                {data.schoolNames.map((school, schoolIdx) => (
                                 <button className="school-btn" key={schoolIdx} onClick={() => handleSchoolClick(school)}>
                                        {school}
                                    </button>
                                ))}
                                </div>
                            </ul>
                        </li>
                    ))}
                </ul>
            </div>
        </div>
    );
}
