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
    const [history, setHistory] = useState([]); // New state for history

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
        updateHistory(question); // Update history when a question is clicked
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
    };

    const updateHistory = (question) => {
        setHistory((prevHistory) => [
            ...prevHistory,
            {
                question,
                description: data.questionDescriptions[question]
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
            <div className="selected-questions">
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
                                        {expandedDescriptions[`${selectedQuestion}-${idx}`] && (
                                            <ul>
                                                {data.schoolNames.map((school, schoolIdx) => (
                                                    <li key={schoolIdx} onClick={() => handleSchoolClick(school)}>
                                                        {school}
                                                    </li>
                                                ))}
                                            </ul>
                                        )}
                                    </div>
                                ))}
                            </div>
                        )}
                    </div>
                )}
            </div>
            <div className="history">
                <h2>History</h2>
                <ul>
                    {history.map((item, index) => (
                        <li key={index}>
                            <h4>{item.question}</h4>
                            <ul>
                                {item.description.map((desc, descIdx) => (
                                    <li key={descIdx}>{desc}</li>
                                ))}
                            </ul>
                        </li>
                    ))}
                </ul>
            </div>
        </div>
    );
}








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
    const [history, setHistory] = useState([]); // New state for history

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
        updateHistory(question); // Update history when a question is clicked
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
    };

    const updateHistory = (question) => {
        setHistory((prevHistory) => [
            ...prevHistory,
            {
                question,
                description: data.questionDescriptions[question]
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
            <div className="selected-questions">
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
                                        {expandedDescriptions[`${selectedQuestion}-${idx}`] && (
                                            <ul>
                                                {data.schoolNames.map((school, schoolIdx) => (
                                                    <li key={schoolIdx} onClick={() => handleSchoolClick(school)}>
                                                        {school}
                                                    </li>
                                                ))}
                                            </ul>
                                        )}
                                    </div>
                                ))}
                            </div>
                        )}
                    </div>
                )}
            </div>
            <div className="history">
                <h2>History</h2>
                <ul>
                    {history.map((item, index) => (
                        <li key={index}>
                            <h4>{item.question}</h4>
                            <ul>
                                {item.description.map((desc, descIdx) => (
                                    <li key={descIdx}>{desc}</li>
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
    max-width: 800px;
    margin: 0 auto;
    padding: 20px;
}

.hamburger-menu {
    cursor: pointer;
    font-size: 24px;
    margin-bottom: 10px;
}

.most-asked-questions {
    display: none;
}

.most-asked-questions.show {
    display: block;
}

.most-asked-questions h4 {
    margin-bottom: 10px;
}

.most-asked-questions ul {
    list-style: none;
    padding: 0;
}

.most-asked-questions li {
    cursor: pointer;
    margin: 5px 0;
    padding: 10px;
    background: #f5f5f5;
    border-radius: 5px;
    transition: background 0.3s ease;
}

.most-asked-questions li:hover {
    background: #e0e0e0;
}

.selected-questions {
    margin-top: 20px;
}

.selected-question {
    margin-bottom: 20px;
}

.question-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    cursor: pointer;
    background: #f0f0f0;
    padding: 10px;
    border-radius: 5px;
    transition: background 0.3s ease;
}

.question-header:hover {
    background: #e0e0e0;
}

.descriptions {
    padding: 10px;
    background: #fafafa;
    border: 1px solid #ddd;
    border-radius: 5px;
    margin-top: 10px;
}

.description {
    cursor: pointer;
    padding: 5px;
    margin: 5px 0;
    background: #f9f9f9;
    border-radius: 5px;
    transition: background 0.3s ease;
}

.description:hover {
    background: #f0f0f0;
}

.description ul {
    list-style: none;
    padding: 0;
    margin-top: 10px;
}

.description li {
    padding: 5px;
    background: #eee;
    margin: 5px 0;
    border-radius: 5px;
    cursor: pointer;
    transition: background 0.3s ease;
}

.description li:hover {
    background: #ddd;
}

.history {
    margin-top: 30px;
    padding: 20px;
    background: #f7f7f7;
    border: 1px solid #ddd;
    border-radius: 5px;
}

.history h2 {
    margin-bottom: 15px;
    font-size: 24px;
}

.history ul {
    list-style: none;
    padding: 0;
}

.history li {
    margin-bottom: 15px;
}

.history li h4 {
    margin: 0;
    font-size: 20px;
}

.history li ul {
    list-style: disc inside;
    margin-top: 10px;
    padding-left: 20px;
}

.history li ul li {
    margin-bottom: 5px;
}
