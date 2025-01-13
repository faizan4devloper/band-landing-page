traceButton.addEventListener('click', function() {
    fetch('https:tracesData', {
        method: 'GET',
        headers: {'Accept': 'application/json'},
    })
    .then(response => response.json())
    .then(data => {
        const traceStepsContainer = document.getElementById('trace-steps');
        traceStepsContainer.innerHTML = ''; // Clear existing trace steps

        data.traces.forEach(trace => {
            const traceStep = document.createElement('div');
            traceStep.className = 'trace-step';
            traceStep.textContent = `Step: ${trace.step}, Description: ${trace.description}`;
            traceStepsContainer.appendChild(traceStep);
        });
    })
    .catch(error => console.error('Error:', error));
});



#chat-content {
    display: flex;
}

#chat-messages {
    width: 70%;
    height: 500px;
    overflow-y: scroll;
    border-right: 1px solid #ccc;
    margin-right: 10px;
}

#trace-steps {
    width: 30%;
    height: 500px;
    overflow-y: scroll;
    background-color: #f9f9f9;
    padding: 10px;
    border-radius: 10px;
}

.trace-step {
    margin-bottom: 10px;
    padding: 10px;
    background-color: #e0e0e0;
    border-radius: 5px;
}
