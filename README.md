import React, { useState } from "react";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faPaperPlane, faUser, faRobot } from "@fortawesome/free-solid-svg-icons";
import axios from "axios";
import Skills from "./Skills";
import JobMatching from "./JobMatching";
import "./ChatWindow.css";

export function ChatWindow() {
  const [messages, setMessages] = useState([]);
  const [conversationStarted, setConversationStarted] = useState(false);
  const [userInput, setUserInput] = useState("");

  const handleSend = async () => {
    if (userInput.trim()) {
      const newMessage = { sender: "user", text: userInput };
      setMessages([...messages, newMessage]);
      setUserInput("");

      if (!conversationStarted) {
        setConversationStarted(true);
      }

      try {
        const response = await axios.post("https://5toybip0v5.execute-api.us-east-1.amazonaws.com/s1/", {
          type: "both",
          query: newMessage.text
        }, {
          headers: {
            'Content-Type': 'application/json'
          }
        });

        console.log(response);
        const botMessage = { sender: "bot", text: "This is a bot response to: " + response.data.user_query_ans };
        setMessages((prevMessages) => [...prevMessages, botMessage]);

      } catch (error) {
        console.error("Error during API call:", error);
        setMessages((prevMessages) => [
          ...prevMessages,
          { sender: "bot", text: "Error: Unable to send message" },
        ]);
      }
    }
  };

  return (
    <div className="chat-window">
      <div className="chat-content">
        <h3>Live Conversation</h3>
        <div className={`chat-messages ${conversationStarted ? 'show' : ''}`}>
          {messages.map((message, index) => (
            <div key={index} className={`chat-message ${message.sender}`}>
              <FontAwesomeIcon icon={message.sender === 'user' ? faUser : faRobot} />
              {message.text}
            </div>
          ))}
        </div>
        <div className="chat-input">
          <input
            type="text"
            value={userInput}
            onChange={(e) => setUserInput(e.target.value)}
            onKeyPress={(e) => e.key === "Enter" && handleSend()}
            placeholder="Type a message..."
          />
          <button onClick={handleSend}>
            <FontAwesomeIcon icon={faPaperPlane} />
          </button>
        </div>
      </div>
      <Skills />
      <JobMatching />
    </div>
  );
}

export default ChatWindow;





import React, { useState } from "react";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faChevronDown, faChevronUp } from "@fortawesome/free-solid-svg-icons";
import "./Skills.css";

function Skills() {
  const [expanded, setExpanded] = useState(false);
  const [suggestions, setSuggestions] = useState("");

  const handleFetchSuggestions = async () => {
    try {
      // Simulating an API call to fetch suggestions
      const response = await fetch("https://api.example.com/skills");
      const data = await response.json();
      setSuggestions(data.suggestions);
    } catch (error) {
      console.error("Error fetching suggestions:", error);
    }
  };

  const toggleExpand = () => {
    setExpanded(!expanded);
    if (!expanded) {
      handleFetchSuggestions();
    }
  };

  return (
    <div className="uploading-section">
      <h3>Skill Improvement and Suggestions</h3>
      <div className="toggle-button" onClick={toggleExpand}>
        <span>{expanded ? "Hide Details" : "Show Details"}</span>
        <FontAwesomeIcon icon={expanded ? faChevronUp : faChevronDown} />
      </div>
      {expanded && suggestions && (
        <div className="content">
          <p>{suggestions}</p>
        </div>
      )}
    </div>
  );
}

export default Skills;




import React, { useState } from "react";
import "./JobMatching.css";

function JobMatching() {
  const [jobMatches, setJobMatches] = useState([]);

  const handleFetchJobMatches = async () => {
    try {
      // Simulating an API call to fetch job matches
      const response = await fetch("https://api.example.com/jobs");
      const data = await response.json();
      setJobMatches(data.jobMatches);
    } catch (error) {
      console.error("Error fetching job matches:", error);
    }
  };

  const handleShowJobMatches = () => {
    handleFetchJobMatches();
  };

  return (
    <div className="preview">
      <h3>Job Recommendations</h3>
      <button onClick={handleShowJobMatches}>Show Job Matches</button>
      {jobMatches.length > 0 && (
        <ul>
          {jobMatches.map((job, index) => (
            <li key={index}>{job}</li>
          ))}
        </ul>
      )}
    </div>
  );
}

export default JobMatching;
