import React, { useState } from "react";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faPaperPlane, faUser, faRobot } from "@fortawesome/free-solid-svg-icons";
import UploadDoc from "./UploadDoc";
import axios from "axios";
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
                  
                "type":"both",  
                "query":newMessage.text
                    // sender: "user",
                    // text: userInput,
                }, {
                    headers: {
                        'Content-Type': 'application/json'
                    }
                });

                console.log(response)
                const botMessage = { sender: "bot", text: "This is a bot response to: " + response.data.user_query_ans };
                const botMessageScreen2 = { sender: "bot", text: "This is a bot response to: " + response.data.skill_suggestion };
                const botMessageScreen3 = { sender: "bot", text: "This is a bot response to: " + response.data.matching_job_urls };

                setMessages((prevMessages) => [...prevMessages, botMessage,botMessageScreen2, botMessageScreen3]);
        
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
                                        <FontAwesomeIcon  icon={message.sender === 'user' ? faUser : faRobot} />

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
      <UploadDoc />
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
      {expanded && (
        <div className="content">
          <p>
            Based on the comparison between the current job/industry requirements for a Data Scientist and your current skillset, I suggest you focus on developing the following key skills to bridge the gap and increase your chances of getting the desired job in the current job market:
          </p>
          <ul>
            <li><strong>Machine Learning and Artificial Intelligence</strong>: Expand your knowledge and expertise in advanced machine learning techniques such as deep learning, reinforcement learning, and natural language processing. This will be crucial for developing intelligent systems and models.</li>
            <li><strong>Big Data Technologies</strong>: Enhance your proficiency in big data frameworks and tools like Hadoop, Spark, and Kafka. This will enable you to handle and process large-scale, complex data efficiently.</li>
            <li><strong>Data Modeling and Architecture</strong>: Develop skills in data modeling, data warehousing, and database design to ensure the data infrastructure can support advanced analytics and decision-making.</li>
            <li><strong>Communication and Presentation Skills</strong>: Strengthen your ability to effectively communicate your findings and recommendations to both technical and non-technical stakeholders. This will be important for translating data insights into business value.</li>
            <li><strong>Domain-Specific Knowledge</strong>: Consider gaining domain expertise in specific industries or areas of interest, such as finance, healthcare, or retail, to better understand the context and requirements of your target roles.</li>
          </ul>
          <p>
            You've already developed a strong foundation in programming, statistics, data visualization, and big data processing, which are all essential skills for a Data Scientist. By focusing on the areas mentioned above, you can further enhance your skillset and increase your chances of securing the desired Data Scientist role.
          </p>
        </div>
      )}
    </div>
  );
}

export default Skills;



import React from 'react';
import "./JobMatching.css";


function JobMatching(){
    return <div className="preview">
        <h3>Job Recommendations</h3>
    </div>
}

export default JobMatching;
