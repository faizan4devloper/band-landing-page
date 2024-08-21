Uncaught Error: Objects are not valid as a React child (found: object with keys {errorMessage, errorType, requestId, stackTrace}). If you meant to render a collection of children, use an array instead.
    at throwOnInvalidObjectType (react-dom.development.js:14887:1)
    at reconcileChildFibers (react-dom.development.js:15828:1)
    at reconcileChildren (react-dom.development.js:19174:1)
    at updateHostComponent (react-dom.development.js:19924:1)
    at beginWork (react-dom.development.js:21618:1)
    at HTMLUnknownElement.callCallback (react-dom.development.js:4164:1)
    at Object.invokeGuardedCallbackDev (react-dom.development.js:4213:1)
    at invokeGuardedCallback (react-dom.development.js:4277:1)
    at beginWork$1 (react-dom.development.js:27451:1)
    at performUnitOfWork (react-dom.development.js:26557:1)
react-dom.development.js:18687 The above error occurred in the <div> component:

    at div
    at div
    at div
    at div
    at Chatbot (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:2481:78)
    at div
    at MainApp (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:647:82)
    at Router (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:50985:15)
    at BrowserRouter (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:48938:5)
    at App

Consider adding an error boundary to your tree to customize error handling behavior.
Visit https://reactjs.org/link/error-boundaries to learn more about error boundaries.






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
      const response = await axios.post('dummy', {
        func: 'generate_response',
        question: input,  // Pass the question to the backend
      });

      console.log(response.data);

      // Get the bot's response text
      const botMessage = { text: response.data, sender: 'bot' };

      // Update the bot's message
      setMessages((prevMessages) =>
        prevMessages.map((msg) =>
          msg.loading ? botMessage : msg
        )
      );
    } catch (error) {
      // Handle error (you might want to display an error message to the user)
      console.error('Error sending message:', error);
      
      // Show an error message in the chat
      setMessages((prevMessages) =>
        prevMessages.map((msg) =>
          msg.loading ? { text: 'Sorry, something went wrong.', sender: 'bot' } : msg
        )
      );
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
                if (e.key === 'Enter') {
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




this is my lambda code
################# Generate Response ###############################################
    if qtext['func']=='generate_response':
        print("Inside generate_response function")
        question=qtext['question']
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
 
        
#         result_trimmed = resp['text'].split('</q>')[1].strip()
#         print("Type-Trimmed Result:",type(result_trimmed))
#         print("Trimmed Result:",result_trimmed)
        return {
                'statusCode': 200,
                'body': result_trimmed
#                 'body': resp
                }
###### Generate Answer Section - END #######################
    #### --- END --- Generate Response Section ##############


this is my react code 
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
      const response = await axios.post('dummy', 
      { func: "generate_response" });
      
      console.log(response.data)

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
correct my react js code

