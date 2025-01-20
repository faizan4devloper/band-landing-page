
const toggleTraceData = async (index) => {
  setMessages((prevMessages) =>
    prevMessages.map((msg, i) => ({
      ...msg,
      isTraceLoading: i === index ? true : msg.isTraceLoading, // Set loading for the specific message
      showTracePanel: i === index ? !msg.showTracePanel : msg.showTracePanel, // Toggle only for the clicked message
    }))
  );

  try {
    const response = await fetch(traceApiEndpoint, {
      method: 'GET',
      headers: { Accept: 'application/json' },
    });

    if (!response.ok) throw new Error('Trace fetch failed');

    const data = await response.json();

    setMessages((prevMessages) =>
      prevMessages.map((msg, i) =>
        i === index
          ? {
              ...msg,
              isTraceLoading: false,
              showTracePanel: true, // Ensure panel is shown
              traceData: data, // Assign trace data
            }
          : msg
      )
    );
  } catch (error) {
    console.error('Trace error:', error);

    setMessages((prevMessages) =>
      prevMessages.map((msg, i) =>
        i === index
          ? { ...msg, isTraceLoading: false, showTracePanel: false }
          : msg
      )
    );
  }
};







<button
  onClick={() => toggleTraceData(index)}
  className={`btn btn-trace ${msg.showTracePanel ? 'active' : ''}`}
  disabled={msg.isTraceLoading}
>
  {msg.isTraceLoading ? (
    <BeatLoader color="#fff" size={12} />
  ) : msg.showTracePanel ? (
    'Hide Trace'
  ) : (
    'Enable Trace'
  )}
</button>
