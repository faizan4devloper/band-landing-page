
  
  import React, { useState, useEffect } from "react";
  import axios from "axios";
  import Modal from "react-modal";
  import styles from "./MainContent.module.css";
  import { useNavigate } from "react-router-dom";
  import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
  import { faSync, faChevronRight } from '@fortawesome/free-solid-svg-icons';
  
  
  import DocumentPreview from "./DocumentPreview";
  import ExtractedContent from "./ExtractedContent";
  import ClaimClassification from "./ClaimClassification";
  import ClaimProcessingStatus from "./ClaimProcessingStatus";
  
  
  const MainContent = ({ message, rows, clid, setRows, staticPreviewUrl, selectedPolicy, uploadedFileName }) => {
    const navigate = useNavigate();
    const [loading, setLoading] = useState(false);

    const [data, setData] = useState(null);
    const [percentage, setPercentage] = useState(null);
    const [isLoading, setIsLoading] = useState(false);
      const [embeddingStatus, setEmbeddingStatus] = useState(null);
    
  
    // Existing useEffects (fetchPercentage and data loading)
    useEffect(() => {
      if (rows.length > 0) {
        handleReload(rows[0].recNum);
      }
    }, [rows]);
    
    const handleDocumentEmbedQueue = async () => {
    if (rows.length === 0) return;

    const currentClaimId = rows[0].recNum;
    const claimFormPath = data?.total_extracted_data 
      ? JSON.parse(data?.total_extracted_data).CLAIM_FORM_DETAILS?.CLAIM_FORM_PATH 
      : null;

    if (!claimFormPath) {
      console.warn('No claim form path found');
      return;
    }

    try {
      const payload = {
        claimid: currentClaimId,
        recnumber: currentClaimId,
        claimformpath: claimFormPath
      };

      const response = await axios.post(
        "https:docembedqueue",
        payload,
      
        { 
          headers: { 
            "Content-Type": "application/json" 
          } 
        }
      );

      console.log('Document Embed Queue Response:', response);
      
      // Optional: Set embedding status
      setEmbeddingStatus(response.data);
    } catch (error) {
      console.error("Failed to call document embed queue:", error);
      setEmbeddingStatus(null);
    }
  };

  // Trigger document embedding when data is loaded
  useEffect(() => {
    if (data && data.total_extracted_data) {
      handleDocumentEmbedQueue();
    }
  }, [data]);

  
useEffect(() => {
  const fetchPercentage = async () => {
    if (rows.length === 0 || !rows[0]?.recNum) return;

    const currentClaimId = rows[0].recNum;
    setIsLoading(true);
    try {
      const response = await axios.post(
        "https://percentage",
        { claimid: currentClaimId },
        { headers: { "Content-Type": "application/json" } }
      );

      if (response.data && response.data.body) {
        let percentageValue = response.data.body.empty_key_perc 
          ? parseFloat(response.data.body.empty_key_perc.replace('%', ''))
          : 0;
          
        setPercentage(percentageValue);
      }
    } catch (error) {
      setPercentage(0);  // Set to 0 if error occurs
    } finally {
      setIsLoading(false);
    }
  };

  fetchPercentage();
}, [rows, data]);

    const handleVerify = (clid, formtype, summary, selectedPolicy) => {
      if (formtype === 'CANCER') {
        selectedPolicy = 'PS391481';
      } else if (formtype === 'HEART') {
        selectedPolicy = 'PS672908';
      } else {
        selectedPolicy = 'PS672908';
      }
  
      navigate('/verify', {
        state: { clid, summary, selectedPolicy, percentage },
      });
    };
  
    const handleReload = async (recNum) => {
      setLoading(true);
      try {
        const payload = {
          tasktype: "FETCH_SINGLE_ACT_CLAIM",
          claimid: recNum,
        };
  
        const response = await axios.post(
          "https:all", 
          payload, 
          { headers: { "Content-Type": "application/json" } }
        );
        
        console.log('Extraction:', response)
        
        setData(response.data.allclaimactdata);
      } catch (error) {
        console.error("Failed to fetch data for RecNum:", recNum, error);
      } finally {
        setLoading(false);
      }
    };
  
    return (
      <div className={styles.mainContentWrapper}>
        {rows.length > 0 && (
          <div className={styles.claimIdDisplay}>
            <h3>Claim ID: {rows[0].recNum}</h3>
          </div>
        )}
        
        <div className={styles.mainContentGrid}>
          <div className={styles.leftColumn}>
            <div className={styles.claimClassificationContainer}>
<ClaimClassification 
  data={data?.total_extracted_data ? JSON.parse(data.total_extracted_data) : null} 
  uploadedFileName={uploadedFileName} 
/>
            </div>
            <div className={styles.documentPreviewContainer}>
              <DocumentPreview staticPreviewUrl={staticPreviewUrl} />
            </div>
            
            
          </div>
          
          
  
          <div className={styles.rightColumn}>
            <div className={styles.percentageSection}>
  {isLoading ? (
    <p>Loading percentage...</p>  // Show loading text or a spinner
  ) : (
    <ClaimProcessingStatus 
      percentage={percentage} 
      isLoading={isLoading} 
    />
  )}
</div>
          
            <div className={styles.extractedContentContainer}>
              <ExtractedContent 
                data={data} 
                loading={loading} 
                rows={rows} 
                handleReload={handleReload}
              />
            </div>
            
            <div className={styles.verifyButtonContainer}>
              <button
                className={styles.verifyButton}
                onClick={() =>
                  handleVerify(
                    clid,
                    data?.total_extracted_data
                      ? JSON.parse(data?.total_extracted_data).CLAIM_FORM_DETAILS?.CLAIM_FORM_TYPE
                      : "No Claim Form Type",
                    data?.total_extracted_data
                      ? JSON.parse(data?.total_extracted_data).CLAIM_FORM_DETAILS?.CLAIM_FORM_DETAILED_SUMMARY
                      : "No Summary Available",
                    selectedPolicy
                  )
                }
                disabled={!rows.length}
              >
                Verify Claim <FontAwesomeIcon icon={faChevronRight}/>
              </button>
            </div>
          </div>
        </div>
      </div>
    );
  };
  
  export default MainContent;





import React, { useState, useEffect, useRef } from 'react';
import axios from 'axios';
import styles from './Chatbot.module.css';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { 
  faPaperPlane, 
  faRobot, 
  faTimes, 
  faMinimize 
} from '@fortawesome/free-solid-svg-icons';

const Chatbot = ({ claimId }) => {
  const [isOpen, setIsOpen] = useState(false);
  const [messages, setMessages] = useState([]);
  const [inputMessage, setInputMessage] = useState('');
  const [isLoading, setIsLoading] = useState(false);
  const messagesEndRef = useRef(null);

  // Initial greeting message
  useEffect(() => {
    setMessages([
      {
        id: 0,
        text: "Hello! I'm your AI Claims Assistant. How can I help you with your claim?",
        sender: 'bot'
      }
    ]);
  }, []);

  // Scroll to bottom of messages
  const scrollToBottom = () => {
    messagesEndRef.current?.scrollIntoView({ behavior: "smooth" });
  };

  useEffect(() => {
    scrollToBottom();
  }, [messages]);

  // Handle sending message
const handleSendMessage = async () => {
  if (!inputMessage.trim()) return;

  const userMessage = {
    id: messages.length + 1,
    text: inputMessage,
    sender: 'user'
  };

  setMessages(prevMessages => [...prevMessages, userMessage]);
  setInputMessage('');
  setIsLoading(true);

  try {
    // Payload matching the structure you specified
    const payload = {
      body: {
        claimid: "CL641743",
        userid: "USER1",
        sessionid: "4013061", 
        userquery: inputMessage
      }
    };

    console.log("Request Payload:", payload);

    const response = await axios.post(
      'https:chatbot', 
      payload,
      {
        headers: {
          'Content-Type': 'application/json',
        },
        timeout: 10000
      }
    );

    console.log("Full API Response:", response);

    // Function to extract bot message
    const extractBotMessage = (responseData) => {
      // Check different possible response structures
      if (responseData.body) {
        // If body is a string, try parsing
        if (typeof responseData.body === 'string') {
          try {
            const parsedBody = JSON.parse(responseData.body);
            return parsedBody.message || 
                   parsedBody.response || 
                   'No response from server';
          } catch {
            return responseData.body;
          }
        }

        // If body is an object
        return responseData.body.message || 
               responseData.body.response || 
               JSON.stringify(responseData.body);
      }

      // Fallback
      return 'No response from server';
    };

    // Extract and set bot message
    const botMessageText = extractBotMessage(response.data);

    const botMessage = {
      id: messages.length + 2,
      text: botMessageText,
      sender: 'bot'
    };

    setMessages(prevMessages => [...prevMessages, botMessage]);

  } catch (error) {
    console.error('Comprehensive Chatbot API Error:', {
      errorMessage: error.message,
      errorResponse: error.response,
      errorRequest: error.request
    });

    // Detailed error handling
    const errorMessage = {
      id: messages.length + 2,
      text: error.response?.data?.message || 
            error.message || 
            'Sorry, there was an error processing your request.',
      sender: 'bot'
    };

    setMessages(prevMessages => [...prevMessages, errorMessage]);
  } finally {
    setIsLoading(false);
  }
};

  // Handle input key press
  const handleKeyPress = (e) => {
    if (e.key === 'Enter') {
      handleSendMessage();
    }
  };

  return (
    <div className={styles.chatbotContainer}>
      {/* Chatbot Trigger Button */}
      {!isOpen && (
        <div 
          className={styles.chatbotTrigger} 
          onClick={() => setIsOpen(true)}
        >
          <FontAwesomeIcon icon={faRobot} />
          <span>AI Assistant</span>
        </div>
      )}

      {/* Chatbot Window */}
      {isOpen && (
        <div className={styles.chatbotWindow}>
          {/* Header */}
          <div className={styles.chatbotHeader}>
            <div className={styles.headerContent}>
              <FontAwesomeIcon icon={faRobot} className={styles.robotIcon} />
              <span>AI Claims Assistant</span>
            </div>
            <div className={styles.headerActions}>
              <button 
                className={styles.closeButton}
                onClick={() => setIsOpen(false)}
              >
                <FontAwesomeIcon icon={faTimes} />
              </button>
            </div>
          </div>

          {/* Messages Container */}
          <div className={styles.messagesContainer}>
            {messages.map((msg) => (
              <div 
                key={msg.id} 
                className={`
                  ${styles.message} 
                  ${msg.sender === 'user' ? styles.userMessage : styles.botMessage}
                `}
              >
                {msg.text}
              </div>
            ))}
            {isLoading && (
              <div className={`${styles.message} ${styles.botMessage}`}>
                Typing...
              </div>
            )}
            <div ref={messagesEndRef} />
          </div>

          {/* Input Area */}
          <div className={styles.inputContainer}>
            <input 
              type="text"
              value={inputMessage}
              onChange={(e) => setInputMessage(e.target.value)}
              onKeyPress={handleKeyPress}
              placeholder="Ask about your claim..."
              className={styles.chatInput}
              disabled={isLoading}
            />
            <button 
              onClick={handleSendMessage} 
              className={styles.sendButton}
              disabled={isLoading}
            >
              <FontAwesomeIcon icon={faPaperPlane} />
            </button>
          </div>
        </div>
      )}
    </div>
  );
};

export default Chatbot;
