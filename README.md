.chat-wrapper {
  display: flex;
  min-height: 100vh;
  background-color: #f5f5f5;
  gap: 20px;
  padding: 20px;
}

.upload-panel {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  width: 200px;
  background: #ffffff;
  border-radius: 16px;
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
  padding: 20px;
}

.upload-plus-icon {
  font-size: 40px;
  color: #5c6bc0;
  cursor: pointer;
}

.upload-wrapper p {
  margin-top: 10px;
  font-size: 16px;
  color: #424242;
}

.chat-container {
  flex: 1;
  background: #ffffff;
  border-radius: 16px;
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
  padding: 20px;
  display: flex;
  flex-direction: column;
}








const [isUploadLoading, setIsUploadLoading] = useState(false);
const [isTraceLoading, setIsTraceLoading] = useState(false);

const handleUpload = async (file) => {
  const reader = new FileReader();

  reader.onload = async (e) => {
    const base64 = e.target.result.split(',')[1];

    try {
      setIsUploadLoading(true); // Set upload loading state
      const response = await fetch(httpEndpoint, {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
          Accept: 'application/json',
        },
        body: JSON.stringify({
          image: base64,
          filename: file.name,
          fileType: file.type,
        }),
      });

      if (!response.ok) throw new Error('Upload failed');

      const data = await response.json();

      if (!data.imageUrl) throw new Error('Invalid upload response');

      if (wsRef.current && wsRef.current.readyState === WebSocket.OPEN) {
        wsRef.current.send(JSON.stringify({
          action: 'sendmessage',
          inputText: data.imageUrl,
          sessionId: sessionId || null,
        }));

        setMessages((prev) => [
          ...prev,
          { type: 'user', text: 'Image uploaded', imageUrl: data.imageUrl },
        ]);
        setTyping(true);
      } else {
        console.warn('WebSocket not open');
      }
    } catch (error) {
      console.error('Upload error:', error);
      setMessages((prev) => [
        ...prev,
        { type: 'user', text: `Error: ${error.message}` },
      ]);
    } finally {
      setIsUploadLoading(false); // Reset upload loading state
    }
  };

  reader.onerror = (error) => {
    console.error('File reading error:', error);
    alert('Failed to read file');
  };

  reader.readAsDataURL(file);
};

const toggleTraceData = async () => {
  if (showTracePanel) {
    setShowTracePanel(false);
    return;
  }
  setIsTraceLoading(true); // Set trace loading state
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
    setIsTraceLoading(false); // Reset trace loading state
  }
};








<button
  onClick={toggleTraceData}
  className={`btn btn-trace ${showTracePanel ? 'active' : ''}`}
  disabled={isTraceLoading}
>
  {isTraceLoading ? (
    <BeatLoader color="#fff" size={12} />
  ) : showTracePanel ? (
    <>
      Hide Trace
      <FontAwesomeIcon icon={faChevronRight} className="btn-icon right-chevron" />
    </>
  ) : (
    <>
      Enable Trace
      <FontAwesomeIcon icon={faChevronRight} className="btn-icon right-chevron" />
    </>
  )}
</button>








<div className="upload-wrapper">
  {isUploadLoading ? (
    <BeatLoader color="#5c6bc0" size={16} />
  ) : (
    <FontAwesomeIcon
      icon={faPlusCircle}
      className="upload-plus-icon"
      onClick={() => document.getElementById('file-upload').click()}
    />
  )}
  <p>Upload File</p>
</div>
