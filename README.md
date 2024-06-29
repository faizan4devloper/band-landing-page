import React, { useState } from "react";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faChevronDown, faChevronUp } from "@fortawesome/free-solid-svg-icons";
import "./Skills.css";

function Skills({ suggestions }) {
  const [expanded, setExpanded] = useState(false);

  const toggleExpand = () => {
    setExpanded(!expanded);
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




import React from "react";
import "./JobMatching.css";

function JobMatching({ jobMatches }) {
  return (
    <div className="preview">
      <h3>Job Recommendations</h3>
      {jobMatches && <p>{jobMatches}</p>}
    </div>
  );
}

export default JobMatching;





import React, { useState } from "react";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faPaperPlane, faUser, faRobot } from "@fortawesome/free-solid-svg-icons";
import UploadDoc from "./UploadDoc";
import Skills from "./Skills";
import JobMatching from "./JobMatching";
import axios from "axios";
import "./ChatWindow.css";

export function ChatWindow() {
  const [messages, setMessages] = useState([]);
  const [conversationStarted, setConversationStarted] = useState(false);
  const [userInput, setUserInput] = useState("");
  const [skillSuggestions, setSkillSuggestions] = useState("");
  const [jobMatches, setJobMatches] = useState("");

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

        setSkillSuggestions(response.data.skill_suggestion);
        setJobMatches(response.data.matching_job_urls);

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
      <Skills suggestions={skillSuggestions} />
      <JobMatching jobMatches={jobMatches} />
      <UploadDoc />
    </div>
  );
}

export default ChatWindow;
