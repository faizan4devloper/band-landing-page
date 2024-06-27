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
}
.chat-container h1 {
    text-align: center;
    margin-top: 14px;
    font-size: 1.5rem;
  line-height: 1.2;
  font-weight: 600;
  background: -webkit-gradient(
    linear,
    left top,
    right top,
    color-stop(-19.51%, #7abef7),
    color-stop(36.51%, #4080f5),
    to(#572ac2)
  );
  background: -webkit-linear-gradient(
    left,
    #7abef7 -19.51%,
    #4080f5 36.51%,
    #572ac2 100%
  );
  background: -o-linear-gradient(
    left,
    #7abef7 -19.51%,
    #4080f5 36.51%,
    #572ac2 100%
  );
  background: linear-gradient(
    90deg,
    #7abef7 -19.51%,
    #4080f5 36.51%,
    #572ac2 100%
  );
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
    /*background-color: #fff;*/
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
.selected-question.expanded p:hover{
    color: #000;
}
.description {
    /*background-color: #f5f5f5;*/
    display: none;
    padding: 8px;
    border-radius: 4px;
    margin: 8px 0;
    cursor: pointer;
    transition: background-color 0.3s ease-in-out, transform 0.3s ease-in-out;
}

.description:hover {
    /*background-color: #ddd;*/
}

.description{
    display: none;
}

.selected-question.expanded .description{
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
    transition: .5s ease;
}

.description ul li:hover{
    background: #e0e0ef;
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


@keyframes fadeIn {
    from {
        opacity: 0;
    }
    to {
        opacity: 1;
    }
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

.history {
    margin-top: 30px;
    padding: 20px;
    background: rgba(255, 255, 255, 0.1);
    border: 1px solid rgba(255, 255, 255, 0.2);
    border-radius: 10px;
    box-shadow: 0 10px 20px rgba(0, 0, 0, 0.2);
}

.history h2 {
    margin-bottom: 15px;
    font-size: 24px;
    color: #fff;
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
    color: #fff;
}

.history li ul {
    list-style: disc inside;
    margin-top: 10px;
    padding-left: 20px;
}

.history li ul li {
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

.history {
    margin-top: 30px;
    padding: 20px;
    background: rgba(255, 255, 255, 0.1);
    border: 1px solid rgba(255, 255, 255, 0.2);
    border-radius: 10px;
    box-shadow: 0 10px 20px rgba(0, 0, 0, 0.2);
}

.history h2 {
    margin-bottom: 15px;
    font-size: 24px;
    color: #fff;
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
    color: #fff;
}

.history li ul {
    list-style: disc inside;
    margin-top: 10px;
    padding-left: 20px;
}

.history li ul li {
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
    const [recentSelections, setRecentSelections] = useState([]);

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
        setRecentSelections((prevSelections) => [
            ...prevSelections.filter(item => item.type !== 'question' || item.value !== question),
            { type: 'question', value: question }
        ]);
    };

    const handleSchoolClick = (school) => {
        onSchoolClick(school);
        setRecentSelections((prevSelections) => [
            ...prevSelections.filter(item => item.type !== 'school' || item.value !== school),
            { type: 'school', value: school }
        ]);
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
            <div className="recent">
                <h2>Recent Selections</h2>
                <ul>
                    {recentSelections.map((item, index) => (
                        <li key={index}>
                            {item.type === 'question' ? (
                                <h4>Question: {item.value}</h4>
                            ) : (
                                <h4>School: {item.value}</h4>
                            )}
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
}

.chat-container h1 {
    text-align: center;
    margin-top: 14px;
    font-size: 1.5rem;
    line-height: 1.2;
    font-weight: 600;
    background: -webkit-gradient(
        linear,
        left top,
        right top,
        color-stop(-19.51%, #7abef7),
        color-stop(36.51%, #4080f5),
        to(#572ac2)
    );
    background: -webkit-linear-gradient(
        left,
        #7abef7 -19.51%,
        #4080f5 36.51%,
        #572ac2 100%
    );
    background: -o-linear-gradient(
        left,
        #7abef7 -19.51%,
        #4080f5 36.51%,
        #572ac2 100%
    );
    background: linear-gradient(
        90deg,
        #7abef7 -19.51%,
        #4080f5 36.51%,
        #572ac2 100%
    );
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

.description {
    display: none;
    padding: 8px;
    border-radius: 4px;
    margin: 8px 0;
    cursor: pointer;
    transition: background-color 0.3s ease-in-out, transform 0.3s ease-in-out;
}

.description:hover {
    background-color: #f5f5f5;
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
    transition: background-color 0.5s ease;
}

.description ul li:hover {
    background: #e0e0ef;
}

.recent {
    margin-top: 30px;
    padding: 20px;
    background: #f7f7f7;
    border: 1px solid #ddd;
    border-radius: 5px;
}

.recent h2 {
    margin-bottom: 15px;
    font-size: 24px;
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
    font-size: 20px;
}

.recent li ul {
    list-style: disc inside;
    margin-top: 10px;
    padding-left: 20px;
}

.recent li ul li {
    margin-bottom: 5px;
}

@keyframes fadeIn {
    from {
        opacity: 0;
    }
    to {
        opacity: 1;
    }
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
    const [recent, setRecent] = useState([]); // New state for history

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
        setRecent((prevHistory) => [
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
            <div className="recent">
                <h2>Recently</h2>
                <ul>
                    {recent.map((item, index) => (
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
    const [recent, setRecent] = useState([]); // New state for recent selections

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
        updateRecent(question); // Update recent selections when a question is clicked
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

    const updateRecent = (question) => {
        setRecent((prevRecent) => [
            ...prevRecent,
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
            <div className="recent">
                <h2>Recent Selections</h2>
                <ul>
                    {recent.map((item, index) => (
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
