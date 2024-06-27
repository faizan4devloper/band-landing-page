import React, { useState } from "react";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faPaperPlane } from "@fortawesome/free-solid-svg-icons";
import "./ChatScreen.css";

export function ChatScreen() {
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

      // Send the user message to the API
      try {
        const response = await fetch("https://api.example.com/chat", {
          method: "POST",
          headers: {
            "Content-Type": "application/json",
          },
          body: JSON.stringify({ message: userInput }),
        });

        if (response.ok) {
          const data = await response.json();
          const botMessage = { sender: "bot", text: data.reply };
          setMessages((prevMessages) => [...prevMessages, botMessage]);
        } else {
          const errorMessage = { sender: "bot", text: "Failed to fetch response from the server." };
          setMessages((prevMessages) => [...prevMessages, errorMessage]);
        }
      } catch (error) {
        const errorMessage = { sender: "bot", text: "An error occurred while fetching the response." };
        setMessages((prevMessages) => [...prevMessages, errorMessage]);
      }
    }
  };

  return (
    <div className="upload-container">
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
          <button onClick={handleSend}>
            <FontAwesomeIcon icon={faPaperPlane} />
          </button>
        </div>
      </div>
    </div>
  );
}




import React, { useState } from "react";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faPaperPlane } from "@fortawesome/free-solid-svg-icons";
import axios from "axios";
import "./ChatScreen.css";

export function ChatScreen() {
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

      // Send the user message to the API
      try {
        const response = await axios.post("https://your-api-endpoint.com/chat", {
          message: userInput,
        });

        const botMessage = { sender: "bot", text: response.data.reply };
        setMessages((prevMessages) => [...prevMessages, botMessage]);
      } catch (error) {
        console.error("Error during API call:", error);
        const errorMessage = { sender: "bot", text: `Error: ${error.message}` };
        setMessages((prevMessages) => [...prevMessages, errorMessage]);
      }
    }
  };

  return (
    <div className="upload-container">
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
          <button onClick={handleSend}>
            <FontAwesomeIcon icon={faPaperPlane} />
          </button>
        </div>
      </div>
    </div>
  );
}

export default ChatScreen;










import React, { useState } from "react";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faPaperPlane } from "@fortawesome/free-solid-svg-icons";
import axios from "axios";
import "./ChatScreen.css";

export function ChatScreen() {
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
        const response = await axios.post("https://your-api-endpoint.com/chat", {
          sender: "user",
          text: userInput,
        });

        const botMessage = { sender: "bot", text: response.data.reply };
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
    <div className="upload-container">
      <div className="chat-content">
        <h3>Live Conversation</h3>
        <div className={`chat-messages ${conversationStarted ? "show" : ""}`}>
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
          <button onClick={handleSend}>
            <FontAwesomeIcon icon={faPaperPlane} />
          </button>
        </div>
      </div>
    </div>
  );
}

export default ChatScreen;









Access to XMLHttpRequest at 'https://2amqzrdnuj.execute-api.us-east-1.amazonaws.com/dev' from origin 'https://dba5e942a9954438ac0fc085398a7f3f.vfs.cloud9.us-east-1.amazonaws.com' has been blocked by CORS policy: Response to preflight request doesn't pass access control check: No 'Access-Control-Allow-Origin' header is present on the requested resource.


bundle.js:543 Error during API call: 
AxiosError
code
: 
"ERR_NETWORK"
config
: 
{transitional: {…}, adapter: Array(3), transformRequest: Array(1), transformResponse: Array(1), timeout: 0, …}
message
: 
"Network Error"
name
: 
"AxiosError"
request
: 
XMLHttpRequest {onreadystatechange: null, readyState: 4, timeout: 0, withCredentials: false, upload: XMLHttpRequestUpload, …}
stack
: 
"AxiosError: Network Error\n    at XMLHttpRequest.handleError (https://dba5e942a9954438ac0fc085398a7f3f.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:100718:14)\n    at Axios.request (https://dba5e942a9954438ac0fc085398a7f3f.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:101156:41)\n    at async handleSend (https://dba5e942a9954438ac0fc085398a7f3f.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:533:26)"
[[Prototype]]
: 
Error
handleSend	@	bundle.js:543






const handleSend = async () => {
    if (userInput.trim()) {
        const newMessage = { sender: "user", text: userInput };
        setMessages([...messages, newMessage]);
        setUserInput("");

        if (!conversationStarted) {
            setConversationStarted(true);
        }

        try {
            const response = await axios.post("https://2amqzrdnuj.execute-api.us-east-1.amazonaws.com/dev/chat", {
                sender: "user",
                text: userInput,
            }, {
                headers: {
                    'Content-Type': 'application/json'
                }
            });

            const botMessage = { sender: "bot", text: response.data.reply };
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
