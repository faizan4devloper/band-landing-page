.btn-trace {
  background-color: #785ce5;
  color: #fff;
  border: none;
  padding: 8px 16px;
  font-size: 14px;
  cursor: pointer;
  transition: background-color 0.3s;
  border-radius: 4px;
}

.btn-trace:hover {
  background-color: #5a43c4;
}

.btn-trace:disabled {
  background-color: #b0a4e0;
  cursor: not-allowed;
}

.btn-trace.active {
  background-color: #4a32a3;
}


import React, { useState } from 'react';
import { BeatLoader } from 'react-spinners';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faChevronRight } from '@fortawesome/free-solid-svg-icons';
import './TraceToggle.css';

const TraceToggle = ({ index, toggleTraceData, isTraceLoading, showTracePanel }) => {
  return (
    <button
      onClick={() => toggleTraceData(index)}
      className={`btn-trace ${showTracePanel ? 'active' : ''}`}
      disabled={isTraceLoading}
    >
      {isTraceLoading ? (
        <BeatLoader color="#fff" size={10} />
      ) : showTracePanel ? (
        <>Hide Trace</>
      ) : (
        <>Enable Trace</>
      )}
    </button>
  );
};

export default TraceToggle;
