import React, { useState } from "react";
import HealthChat from "./HealthChat";
import GraphicalInsight from "./GraphicalInsight";
import Details from "./Details";

function HealthAdvisor() {
  const [graphData, setGraphData] = useState("");
  const [gpDetails, setGpDetails] = useState("");

  const onHealthChatResponse = (response) => {
    console.log("HealthChat Response:", response);
    setGraphData(response.graph_data);
    setGpDetails(response.gp_details);
  };

  return (
    <div className="health-advisor">
      <HealthChat onHealthChatResponse={onHealthChatResponse} />
      <GraphicalInsight graph={graphData} />
      <Details details={gpDetails} />
    </div>
  );
}

export default HealthAdvisor;



import React, { useState } from "react";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faPaperPlane, faUser, faRobot } from "@fortawesome/free-solid-svg-icons";
import axios from "axios";
import "./HealthChat.css";

function HealthChat({ onHealthChatResponse }) {
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
        const response = await axios.post("https://vl6hme4evj.execute-api.us-east-1.amazonaws.com/dev/", {
          query: newMessage.text
        }, {
          headers: {
            'Content-Type': 'application/json'
          }
        });

        console.log("API Response:", response.data);
        const botMessage = { sender: "bot", text: "This is a bot response to: " + response.data.query_response };
        setMessages((prevMessages) => [...prevMessages, botMessage]);

        onHealthChatResponse(response.data);

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
    </div>
  );
}

export default HealthChat;




import React from "react";

function GraphicalInsight({ graph }) {
  console.log("GraphicalInsight Props:", graph);
  return (
    <div className="graphical-insight">
      <h3>Graphical Insight</h3>
      {graph ? <div>{/* Render your graph here using the graph data */}</div> : <p>No data available</p>}
    </div>
  );
}

export default GraphicalInsight;






import React from "react";

function Details({ details }) {
  console.log("Details Props:", details);
  return (
    <div className="details">
      <h3>Details</h3>
      {details ? <div>{/* Render your details here using the details data */}</div> : <p>No data available</p>}
    </div>
  );
}

export default Details;
