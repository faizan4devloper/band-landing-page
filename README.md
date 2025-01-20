import React from 'react';
import { BeatLoader } from 'react-spinners';

const TraceToggle = ({ index, msg, toggleTraceData }) => (
  <span className="trace-button-container">
    <button
      onClick={() => toggleTraceData(index)}
      className={`btn-trace ${msg.showTracePanel ? 'active' : ''}`}
      disabled={msg.isTraceLoading}
    >
      {msg.isTraceLoading ? (
        <BeatLoader color="#fff" size={10} />
      ) : msg.showTracePanel ? (
        <>Hide Trace</>
      ) : (
        <>Enable Trace</>
      )}
    </button>
  </span>
);

export default TraceToggle;



import React, { useState, useEffect, useRef, useCallback } from 'react';
import { BeatLoader } from 'react-spinners';
import { motion, AnimatePresence } from 'framer-motion';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faPaperPlane, faPlus, faRobot, faUser } from '@fortawesome/free-solid-svg-icons';
import TracePanel from './TracePanel';
import TraceToggle from './TraceToggle'; // Import the new component
import './Chat.css';

const Chat = () => {
  // State and logic here...

  const toggleTraceData = async (index) => {
    setIsTraceLoading(true);

    setMessages((prevMessages) =>
      prevMessages.map((msg, i) => ({
        ...msg,
        showTracePanel: i === index ? !msg.showTracePanel : msg.showTracePanel,
      }))
    );

    if (showTracePanel) {
      setShowTracePanel(false);
      return;
    }

    setIsTraceLoading(true);
    try {
      const response = await fetch(traceApiEndpoint, {
        method: 'GET',
        headers: { Accept: 'application/json' },
      });

      if (!response.ok) throw new Error('Trace fetch failed');

      const data = await response.json();
      setTraceData(data);
      setIsTraceEnabled(true);
      setShowTracePanel(true);
    } catch (error) {
      console.error('Trace error:', error);
      setTraceData([]);
      setShowTracePanel(false);
    } finally {
      setIsTraceLoading(false);
    }
  };

  return (
    <div className="chat-wrapper">
      {/* Left Panel - Chatbot */}
      <div className="chatbot-panel">
        {/* Chat messages */}
        <div className="chat-messages">
          <AnimatePresence>
            {messages.map((msg, index) => (
              <motion.div
                key={index}
                className={`message ${msg.type === 'user' ? 'user-message' : 'bot-message'}`}
                initial={{ opacity: 0, y: 20 }}
                animate={{ opacity: 1, y: 0 }}
                transition={{ duration: 0.3 }}
              >
                <div className="message-icon">
                  <FontAwesomeIcon icon={msg.type === 'user' ? faUser : faRobot} className={`${msg.type}-icon`} />
                </div>
                <div className="message-text">
                  {msg.text}
                  {msg.type === 'bot' && (
                    <TraceToggle 
                      index={index} 
                      msg={msg} 
                      toggleTraceData={toggleTraceData} 
                    />
                  )}
                </div>
              </motion.div>
            ))}
          </AnimatePresence>
          {typing && (
            <div className="typing-indicator">
              <BeatLoader color="#785ce5" size={12} />
            </div>
          )}
          <div ref={messagesEndRef} />
        </div>

        {/* Chat input */}
        <div className="chat-input-container">
          <div className="input-wrapper">
            <div className="input-icons">
              <FontAwesomeIcon 
                icon={faPlus} 
                className="upload-icon" 
                onClick={() => document.getElementById('image-upload').click()}
              />
            </div>
            <input
              type="text"
              value={input}
              onChange={(e) => setInput(e.target.value)}
              onKeyDown={handleKeyPress}
              placeholder="Type your message..."
              className="chat-input"
            />
            <input
              type="file"
              id="image-upload"
              onChange={(e) => handleUpload(e.target.files[0])}
              style={{ display: 'none' }}
            />
            <button onClick={sendMessage} className="btn btn-send">
              <FontAwesomeIcon icon={faPaperPlane} />
            </button>
          </div>
        </div>
      </div>

      {/* Trace Panel */}
      {showTracePanel && (
        <TracePanel 
          isLoading={isLoading}
          traceData={traceData}
          isTraceEnabled={isTraceEnabled}
          onClose={() => setShowTracePanel(false)}
        />
      )}
    </div>
  );
};

export default Chat;
