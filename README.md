import React from "react";
import { Line } from "react-chartjs-2";

const AdmissionChart = () => {
  const data = {
    labels: ["2020-2021", "2021-2022", "2022-2023", "2023-2024", "2024-2025"],
    datasets: [
      {
        label: "# of Applications Received",
        data: [800, 1000, 1200, 1500, 2000],
        borderColor: "rgba(75, 192, 192, 1)",
        backgroundColor: "rgba(75, 192, 192, 0.2)",
        borderWidth: 1,
      },
      {
        label: "# of Applications Accepted",
        data: [250, 300, 300, 350, 380],
        borderColor: "rgba(255, 99, 132, 1)",
        backgroundColor: "rgba(255, 99, 132, 0.2)",
        borderWidth: 1,
      },
    ],
  };

  const options = {
    scales: {
      yAxes: [
        {
          ticks: {
            beginAtZero: true,
          },
        },
      ],
    },
  };

  return (
    <div className="chart">
      <h5>Admission Trend Chart</h5>
      <Line data={data} options={options} />
    </div>
  );
};

export default AdmissionChart;
import React from "react";
import { Line } from "react-chartjs-2";

const AppealChart = () => {
  const data = {
    labels: ["2020-2021", "2021-2022", "2022-2023", "2023-2024", "2024-2025"],
    datasets: [
      {
        label: "# of Appeals Received",
        data: [20, 30, 35, 40, 43],
        borderColor: "rgba(75, 192, 192, 1)",
        backgroundColor: "rgba(75, 192, 192, 0.2)",
        borderWidth: 1,
      },
      {
        label: "# of Appeals Accepted",
        data: [4, 6, 8, 10, 12],
        borderColor: "rgba(255, 99, 132, 1)",
        backgroundColor: "rgba(255, 99, 132, 0.2)",
        borderWidth: 1,
      },
    ],
  };

  const options = {
    scales: {
      yAxes: [
        {
          ticks: {
            beginAtZero: true,
          },
        },
      ],
    },
  };

  return (
    <div className="chart">
      <h5>Appeal Trend Chart</h5>
      <Line data={data} options={options} />
    </div>
  );
};

export default AppealChart;



import React, { useState } from "react";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faAngleRight, faAngleDown } from "@fortawesome/free-solid-svg-icons";
import "./Insights.css";
import AdmissionChart from "./AdmissionChart";
import AppealChart from "./AppealChart";

const Insights = ({ selectedSchool }) => {
  const [popupContent, setPopupContent] = useState(null);
  const [expandedSection, setExpandedSection] = useState(null);

  const handleExpand = (section) => {
    setExpandedSection((prevSection) => (prevSection === section ? null : section));
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
                <p onClick={() => setPopupContent("PDF link")}>
                  PDF link (pop up view)
                </p>
              </div>
            )}
          </div>
        </div>
      )}
      {popupContent === "parentReviews" && (
        <div className="popup">
          <h4>Parent Review Popup</h4>
          {/* Add content for parent reviews popup */}
          <button onClick={closePopup}>Close</button>
        </div>
      )}
    </div>
  );
};

export default Insights;

































import React, { useState } from "react";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faPaperPlane } from "@fortawesome/free-solid-svg-icons";
import "./ChatScreen.css"

export function ChatScreen(){
    const [messages, setMessages] = useState([]);
    const [conversationStarted, setConversationStarted] = useState(false)
    const [userInput, setUserInput] = useState("");
    const handleSend = () => {
        if (userInput.trim()) {
            const newMessage = { sender: "user", text: userInput };
            setMessages([...messages, newMessage]);
            setUserInput("");
            if(!conversationStarted){
                setConversationStarted(true)
            }
            // Simulate bot response
            setTimeout(() => {
                const botMessage = { sender: "bot", text: "This is a bot response." };
                setMessages(prevMessages => [...prevMessages, botMessage]);
            }, 1000);
        }
    };
    

    
    return <div className="upload-container">
       <div className="chat-content">
                <h3>Live Conversation</h3>
                <div className={`chat-messages ${conversationStarted ? 'show' : ''}`}>
    {messages.map((message, index) => (
        <div key={index} className={`chat-message ${message.sender}`}>
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
                    <button onClick={handleSend}><FontAwesomeIcon icon={faPaperPlane} /></button>
                </div>
            </div>
    </div>
}
