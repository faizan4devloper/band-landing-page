Chatbot.js:54 Error sending message: 
AxiosError
code
: 
"ERR_BAD_RESPONSE"
config
: 
{transitional: {…}, adapter: Array(3), transformRequest: Array(1), transformResponse: Array(1), timeout: 0, …}
message
: 
"Request failed with status code 502"
name
: 
"AxiosError"
request
: 
XMLHttpRequest {onreadystatechange: null, readyState: 4, timeout: 0, withCredentials: false, upload: XMLHttpRequestUpload, …}
response
: 
{data: {…}, status: 502, statusText: '', headers: AxiosHeaders, config: {…}, …}
stack
: 
"AxiosError: Request failed with status code 502\n    at settle (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:89423:12)\n    at XMLHttpRequest.onloadend (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:88086:66)\n    at Axios.request (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:88576:41)\n    at async sendMessage (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:2516:24)"
[[Prototype]]
: 
Error







import React, { useState, useEffect } from 'react';
import axios from 'axios'; // Import Axios
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faPaperPlane, faRobot, faTimes } from '@fortawesome/free-solid-svg-icons';
import { faUser } from '@fortawesome/free-regular-svg-icons';
import { BeatLoader } from 'react-spinners';
import styles from './Chatbot.module.css';

const Chatbot = () => {
  const [isOpen, setIsOpen] = useState(false);
  const [messages, setMessages] = useState([]);
  const [input, setInput] = useState('');
  const [loading, setLoading] = useState(false);

  useEffect(() => {
    if (isOpen) {
      setMessages([{ text: 'Hello! How can I assist you today?', sender: 'bot' }]);
    }
  }, [isOpen]);

  const toggleChatbot = () => {
    setIsOpen(!isOpen);
  };

  const sendMessage = async () => {
    if (input.trim() === '') return;

    // Add user message
    const userMessage = { text: input, sender: 'user' };
    setMessages([...messages, userMessage]);
    setInput('');
    setLoading(true);

    // Add a loading indicator as a bot message before the actual response
    setMessages((prevMessages) => [
      ...prevMessages,
      { text: '', sender: 'bot', loading: true },
    ]);

    try {
      // Send user message to your API
      const response = await axios.post('YOUR_API_ENDPOINT', { body: JSON.stringify({ query: input }) });

      // Parse the response directly from the body
      const botMessage = { text: response.data, sender: 'bot' };

      // Update the messages state with the actual bot response
      setMessages((prevMessages) =>
        prevMessages.map((msg, index) =>
          index === prevMessages.length - 1 ? botMessage : msg
        )
      );
    } catch (error) {
      console.error('Error sending message:', error);
      setMessages((prevMessages) => [
        ...prevMessages,
        { text: 'Sorry, something went wrong. Please try again later.', sender: 'bot' },
      ]);
    } finally {
      setLoading(false);
    }
  };

  return (
    <>
      <div className={styles.chatbotIcon} onClick={toggleChatbot}>
        <FontAwesomeIcon icon={faRobot} />
      </div>
      
      {isOpen && (
        <div className={styles.chatbotContainer}>
          <div className={styles.chatbotHeader}>
            <button onClick={toggleChatbot} className={styles.closeButton}>
              <FontAwesomeIcon icon={faTimes} />
            </button>
          </div>

          <div className={styles.chatbotMessages}>
            {messages.map((message, index) => (
              <div
                key={index}
                className={
                  message.sender === 'user' ? styles.userMessage : styles.botMessage
                }
              >
                <FontAwesomeIcon
                  icon={message.sender === 'user' ? faUser : faRobot}
                  className={styles.icon}
                />
                <div className={styles.messageText}>
                  {message.loading ? (
                    <BeatLoader color="#5f1ec1" size={8} />
                  ) : (
                    message.text
                  )}
                </div>
              </div>
            ))}
          </div>

          <div className={styles.chatbotInput}>
            <input
              type="text"
              value={input}
              onChange={(e) => setInput(e.target.value)}
              onKeyDown={(e) => {
                if (e.key === "Enter") {
                  sendMessage();
                }
              }}
              placeholder="Type your message..."
              disabled={loading}
            />
            <button onClick={sendMessage} disabled={loading} className={styles.sendButton}>
              <FontAwesomeIcon icon={faPaperPlane} />
            </button>
          </div>
        </div>
      )}
    </>
  );
};

export default Chatbot;



















import json
import re

def lambda_handler(event, context):
    print("------------------ NEW event:", type(event), event)
    
    data_string = event["body"]
    print("event[body]", data_string, type(data_string))
    
    qtext = json.loads(data_string)
    print("qtext=json.loads(data_string):", type(qtext), qtext)
       
    print("qtext['query']:=", qtext['query'])
    
    # Generate response section
    if 'generate_response' == 'generate_response':
        print("Inside generate_response function")
        question = qtext['query']
        print("Question Received:", question)
        
        # Mockup for bedrock_chain function
        resp_text = f"Mocked response to the question: {question}"  # Replace this with your actual logic
        
        resp = json.dumps({"text": resp_text})
        resp = json.loads(resp)
        
        # Clean up response
        result_trimmed = re.sub(r'<q>.*?</q>', '', resp['text'])
        if len(result_trimmed) < 100:
            result_trimmed = (resp['text'].replace('<q>', '')).replace('</q>', '')
        
        headers = {
            'Access-Control-Allow-Origin': '*',
            'Access-Control-Allow-Headers': '*',
            'Access-Control-Allow-Methods': '*',
        }
        
        return {
            'statusCode': 200,
            'headers': headers,
            'body': json.dumps({'reply': result_trimmed})  # Wrap the response in 'reply' key
        }




import React, { useState, useEffect } from 'react';
import axios from 'axios'; // Import Axios
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faPaperPlane, faRobot, faTimes } from '@fortawesome/free-solid-svg-icons';
import { faUser } from '@fortawesome/free-regular-svg-icons';
import { BeatLoader } from 'react-spinners';
import styles from './Chatbot.module.css';

const Chatbot = () => {
  const [isOpen, setIsOpen] = useState(false);
  const [messages, setMessages] = useState([]);
  const [input, setInput] = useState('');
  const [loading, setLoading] = useState(false);

  useEffect(() => {
    if (isOpen) {
      setMessages([{ text: 'Hello! How can I assist you today?', sender: 'bot' }]);
    }
  }, [isOpen]);

  const toggleChatbot = () => {
    setIsOpen(!isOpen);
  };

  const sendMessage = async () => {
    if (input.trim() === '') return;

    // Add user message
    const userMessage = { text: input, sender: 'user' };
    setMessages([...messages, userMessage]);
    setInput('');
    setLoading(true);

    // Add a loading indicator as a bot message before the actual response
    setMessages((prevMessages) => [
      ...prevMessages,
      { text: '', sender: 'bot', loading: true },
    ]);

    try {
      // Send user message to your API
      const response = await axios.post('YOUR_API_ENDPOINT', { body: JSON.stringify({ query: input }) });
      
      // Parse the response correctly
      const botMessage = { text: response.data.reply, sender: 'bot' };

      // Update the messages state with the actual bot response
      setMessages((prevMessages) =>
        prevMessages.map((msg, index) =>
          index === prevMessages.length - 1 ? botMessage : msg
        )
      );
    } catch (error) {
      console.error('Error sending message:', error);
      setMessages((prevMessages) => [
        ...prevMessages,
        { text: 'Sorry, something went wrong. Please try again later.', sender: 'bot' },
      ]);
    } finally {
      setLoading(false);
    }
  };

  return (
    <>
      <div className={styles.chatbotIcon} onClick={toggleChatbot}>
        <FontAwesomeIcon icon={faRobot} />
      </div>
      
      {isOpen && (
        <div className={styles.chatbotContainer}>
          <div className={styles.chatbotHeader}>
            <button onClick={toggleChatbot} className={styles.closeButton}>
              <FontAwesomeIcon icon={faTimes} />
            </button>
          </div>

          <div className={styles.chatbotMessages}>
            {messages.map((message, index) => (
              <div
                key={index}
                className={
                  message.sender === 'user' ? styles.userMessage : styles.botMessage
                }
              >
                <FontAwesomeIcon
                  icon={message.sender === 'user' ? faUser : faRobot}
                  className={styles.icon}
                />
                <div className={styles.messageText}>
                  {message.loading ? (
                    <BeatLoader color="#5f1ec1" size={8} />
                  ) : (
                    message.text
                  )}
                </div>
              </div>
            ))}
          </div>

          <div className={styles.chatbotInput}>
            <input
              type="text"
              value={input}
              onChange={(e) => setInput(e.target.value)}
              onKeyDown={(e) => {
                if (e.key === "Enter") {
                  sendMessage();
                }
              }}
              placeholder="Type your message..."
              disabled={loading}
            />
            <button onClick={sendMessage} disabled={loading} className={styles.sendButton}>
              <FontAwesomeIcon icon={faPaperPlane} />
            </button>
          </div>
        </div>
      )}
    </>
  );
};

export default Chatbot;





def lambda_handler(event, context):  # Handler
    
    print("------------------ NEW event:",type(event),event)
    
    data_string = event["body"]
#     print("event[body]",data_string,type(data_string))
#     print("\n","Lambda Handler context:",type(context),context)
#     qtext = json.loads(data_string)
#     print("qtext=json.loads(data_string) :",type(qtext),qtext)
    print("event[body]",data_string,type(data_string))
    #print("\n","Lambda Handler context:",type(context),context)
    qtext = json.loads(data_string)
    print("qtext=json.loads(data_string) :",type(qtext),qtext)
       
    qtext=json.loads(data_string)
    #print(event['body']['question'])
    #qtext=jsninp['type']
    #
    #{'type': 'query_only', 'query': 'How to get visas for my partner and children to live in the UK has context menu'}
    #
    print("\n","qtext:=",qtext)
    print("qtext['query']:=",qtext['query'])
    
    ###### Generate Response Section ########################
    
    ################# Generate Response ###############################################
    if 'generate_response'=='generate_response':
        print("Inside generate_response function")
        question=qtext['query']
        print("Question Recieved:",question)
        
        print("Calling bedrock_chain() , To Declare required items, collection retirver, langchain qa retrival chai etc")        
        
        collec_name=vector_store_name
#         index_name="portal-test-index"
        index_name=index_name_orig
        print(f"Using Collection= {collec_name} & Index= {index_name} to answer user's Query")
        
        chain=bedrock_chain(collec_name,index_name)
        
        print("Recieved response from bedrock_chain() as a qa/Chain:",chain)
        
        print("Now we are running the Chain passing the user's questions")
        
        print(f"Passing question to chain to get response:{question}") 
        
        resp=chain.run({"question": question})
          
        print("Got respose from chain.run() in resp:",resp) 
        
        print("type: resp",type(resp))
        
        print("Before Json.loads",resp)
        
        resp=json.dumps({"text":resp})
        print("json.dumps:",resp)
        resp=json.loads(resp)
        print("After Json.loads",resp)
        
        print("Returning to resp-text",resp['text'])
        print("Type of resp[text]:",type(resp['text']))
#         result_trimmed = re.sub(r'<q>.*?</q>', '', resp['text']) # old
        # New
        result_trimmed = re.sub(r'<q>.*?</q>', '', resp['text'])
        if len(result_trimmed)<100:
            print("hi")
            result_trimmed=(resp['text'].replace('<q>','')).replace('</q>','')
    #result_trimmed=.replace('<q>','')
        else:
            result_trimmed
          ## New --- END  
        headers = {
            'Access-Control-Allow-Origin': '*',  # Replace with your client's origin
            'Access-Control-Allow-Headers': '*',
            'Access-Control-Allow-Methods': '*',  # Adjust based on the allowed methods
        }
        return {
                'statusCode': 200,
                'headers': headers,
                'body': result_trimmed
                #'body': json.dumps(user_query_ans':query_ans')
                                   #                 'body': resp
                }
###### Generate Answer Section - END #######################
    #### --- END --- Generate Response Section ##############
    
 





import React, { useState, useEffect } from 'react';
import axios from 'axios'; // Import Axios
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faPaperPlane, faRobot, faTimes } from '@fortawesome/free-solid-svg-icons';
import { faUser } from '@fortawesome/free-regular-svg-icons';
import { BeatLoader } from 'react-spinners';
import styles from './Chatbot.module.css';

const Chatbot = () => {
  const [isOpen, setIsOpen] = useState(false);
  const [messages, setMessages] = useState([]);
  const [input, setInput] = useState('');
  const [loading, setLoading] = useState(false);

  useEffect(() => {
    if (isOpen) {
      setMessages([{ text: 'Hello! How can I assist you today?', sender: 'bot' }]);
    }
  }, [isOpen]);

  const toggleChatbot = () => {
    setIsOpen(!isOpen);
  };

  const sendMessage = async () => {
    if (input.trim() === '') return;

    // Add user message
    const userMessage = { text: input, sender: 'user' };
    setMessages([...messages, userMessage]);
    setInput('');
    setLoading(true);

    // Add a loading indicator as a bot message before the actual response
    setMessages((prevMessages) => [
      ...prevMessages,
      { text: '', sender: 'bot', loading: true },
    ]);

    try {
      // Send user message to your API
      const response = await axios.post('YOUR_API_ENDPOINT', { message: input });

      // Simulate bot response after a delay
      const botMessage = { text: response.data.reply, sender: 'bot' };

      setMessages((prevMessages) =>
        prevMessages.map((msg) =>
          msg.loading ? botMessage : msg
        )
      );
    } catch (error) {
      // Handle error (you might want to display an error message to the user)
      console.error('Error sending message:', error);
    } finally {
      setLoading(false);
    }
  };

  return (
    <>
      <div className={styles.chatbotIcon} onClick={toggleChatbot}>
        <FontAwesomeIcon icon={faRobot} />
      </div>
      
      {isOpen && (
        <div className={styles.chatbotContainer}>
          <div className={styles.chatbotHeader}>
            <button onClick={toggleChatbot} className={styles.closeButton}>
              <FontAwesomeIcon icon={faTimes} />
            </button>
          </div>

          <div className={styles.chatbotMessages}>
            {messages.map((message, index) => (
              <div
                key={index}
                className={
                  message.sender === 'user' ? styles.userMessage : styles.botMessage
                }
              >
                <FontAwesomeIcon
                  icon={message.sender === 'user' ? faUser : faRobot}
                  className={styles.icon}
                />
                <div className={styles.messageText}>
                  {message.loading ? (
                    <BeatLoader color="#5f1ec1" size={8} />
                  ) : (
                    message.text
                  )}
                </div>
              </div>
            ))}
          </div>

          <div className={styles.chatbotInput}>
            <input
              type="text"
              value={input}
              onChange={(e) => setInput(e.target.value)}
              onKeyDown={(e) => {
                if (e.key === "Enter") {
                  sendMessage();
                }
              }}
              placeholder="Type your message..."
              disabled={loading}
            />
            <button onClick={sendMessage} disabled={loading} className={styles.sendButton}>
              <FontAwesomeIcon icon={faPaperPlane} />
            </button>
          </div>
        </div>
      )}
    </>
  );
};

export default Chatbot;







import React, { useState } from "react";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faPaperPlane, faUser, faRobot, faTimes } from "@fortawesome/free-solid-svg-icons";
import axios from "axios";
import "./Chatbot.css"; // Assume you have a CSS file for styling
import { Loader } from "./Loader"; // Assume you have a Loader component
import UploadDoc from "./UploadDoc"; // Assume you have a document upload component

export function Chatbot() {
  const [messages, setMessages] = useState([]);
  const [conversationStarted, setConversationStarted] = useState(false);
  const [input, setInput] = useState("Based on my resume, is there a job in the current market? Also, help me out on how to apply for a visa for my family.");
  const [loading, setLoading] = useState(false);

  const handleSend = async () => {
    if (input.trim()) {
      const newMessage = { sender: "user", text: input };
      setMessages([...messages, newMessage]);
      setInput("");

      if (!conversationStarted) {
        setConversationStarted(true);
      }

      setLoading(true);

      try {
        const response = await axios.post("https://5toybip0v5.execute-api.us-east-1.amazonaws.com/s1/", {
          func: "generate_response",
          question: newMessage.text
        }, {
          headers: {
            'Content-Type': 'application/json'
          }
        });

        console.log(response);
        const botMessage = { sender: "bot", text: response.data.body };
        setMessages((prevMessages) => [...prevMessages, botMessage]);

      } catch (error) {
        console.error("Error during API call:", error);
        setMessages((prevMessages) => [
          ...prevMessages,
          { sender: "bot", text: "Error: Unable to send message" },
        ]);
      } finally {
        setLoading(false);
      }
    }
  };

  return (
    <div className="chatbot">
      <div className="chatbot-header">
        <h3>Live Conversation</h3>
        <button className="close-button">
          <FontAwesomeIcon icon={faTimes} />
        </button>
      </div>
      <div className={`chatbot-messages ${conversationStarted ? 'show' : ''}`}>
        {messages.map((message, index) => (
          <div key={index} className={`chatbot-message ${message.sender}`}>
            <FontAwesomeIcon icon={message.sender === 'user' ? faUser : faRobot} />
            {message.text}
          </div>
        ))}
      </div>
      <div className="chatbot-input">
        <input
          type="text"
          value={input}
          onChange={(e) => setInput(e.target.value)}
          onKeyPress={(e) => e.key === "Enter" && handleSend()}
          placeholder="Type a message..."
          disabled={loading}
        />
        {loading ? <Loader /> : (
          <button onClick={handleSend}>
            <FontAwesomeIcon icon={faPaperPlane} />
          </button>
        )}
      </div>
      <UploadDoc />
    </div>
  );
}

export default Chatbot;




import React, { useState } from "react";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faPaperPlane, faUser, faRobot } from "@fortawesome/free-solid-svg-icons";
import axios from "axios";
import "./ChatWindow.css";
import { Loader } from "./Loader";

import UploadDoc from "./UploadDoc"

export function ChatWindow({ onChatResponse }) {
  const [messages, setMessages] = useState([]);
  const [conversationStarted, setConversationStarted] = useState(false);
  const [userInput, setUserInput] = useState("Based on my resume, is there a job in current market? Also help me out on how to apply visa for my family");
      const [loading, setLoading] = useState(false);


  const handleSend = async () => {
    if (userInput.trim()) {
      const newMessage = { sender: "user", text: userInput };
      setMessages([...messages, newMessage]);
      setUserInput("");

      if (!conversationStarted) {
        setConversationStarted(true);
      }
      
                        setLoading(true);


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
        const botMessage = { sender: "bot", text:response.data.user_query_ans };
        setMessages((prevMessages) => [...prevMessages, botMessage]);

        // Pass the response data to the parent component
        onChatResponse(response.data);

      } catch (error) {
        console.error("Error during API call:", error);
        setMessages((prevMessages) => [
          ...prevMessages,
          { sender: "bot", text: "Error: Unable to send message" },
        ]);
      }
      finally {
        setLoading(false);
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
            disabled={loading}
          />
          {loading ? <Loader /> : (
            <button onClick={handleSend}>
              <FontAwesomeIcon icon={faPaperPlane} />
            </button>
          )}
        </div>
        <UploadDoc/>
      </div>
    </div>
  );
}

export default ChatWindow;
