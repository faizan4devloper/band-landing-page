/* Main container */
.formDisplay {
    background: linear-gradient(135deg, #1e293b, #334155);
    padding: 30px;
    height: 100%;
    width: 100%;
    overflow-y: auto;
    display: flex;
    justify-content: center;
    align-items: center;
    font-family: 'Arial', sans-serif;
    animation: fadeIn 0.5s ease-in-out;
}

/* Grid layout for cards */
.gridContainer {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
    gap: 20px;
    width: 100%;
    max-width: 1200px; /* Ensure a max width for better spacing */
}

/* Card styles */
.card {
    background: linear-gradient(135deg, #ffffff, #e2e8f0);
    border-radius: 12px;
    box-shadow: 0 6px 20px rgba(0, 0, 0, 0.1);
    transition: transform 0.4s ease, box-shadow 0.4s ease;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    overflow: hidden; /* Ensure it handles overflow properly */
    min-height: 300px; /* Stretching the card to fill vertically */
}

.card:hover {
    transform: scale(1.05);
    box-shadow: 0 12px 28px rgba(0, 0, 0, 0.25);
    transition: transform 0.3s ease, box-shadow 0.3s ease;
}

/* Card header */
.cardHeader {
    background-color: #2563eb;
    color: white;
    padding: 15px;
    font-size: 1.2rem;
    font-weight: bold;
    text-transform: capitalize;
    display: flex;
    align-items: center;
    gap: 10px;
    border-top-left-radius: 12px;
    border-top-right-radius: 12px;
    box-shadow: inset 0 -2px 10px rgba(0, 0, 0, 0.1);
}

.cardIcon {
    color: #f8fafc;
    transition: transform 0.4s ease;
}

.card:hover .cardIcon {
    transform: rotate(360deg);
}

/* Card content */
.cardContent {
    padding: 20px;
    font-size: 1rem;
    color: #374151;
    flex-grow: 1; /* Stretch the content area */
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
}

/* Next button */
.nextBtn {
    margin-top: 20px;
    padding: 0.75rem 2rem;
    font-size: 1.1rem;
    color: #fff;
    background-color: #2563eb;
    border: none;
    border-radius: 30px;
    cursor: pointer;
    transition: background 0.3s ease, box-shadow 0.3s ease;
    box-shadow: 0 6px 12px rgba(0, 0, 0, 0.3);
    justify-self: center;
}

.nextBtn:hover {
    background-color: #3b82f6;
    transform: scale(1.05);
}

.nextIcon {
    margin-left: 10px;
    transition: transform 0.3s ease;
}

.nextBtn:hover .nextIcon {
    transform: translateX(5px);
}

/* Loading and select messages */
.loadingMessage,
.selectMessage {
    font-size: 1.5rem;
    color: #e2e8f0;
    text-align: center;
    margin-top: 20px;
}

/* Animations */
@keyframes fadeIn {
    from {
        opacity: 0;
    }
    to {
        opacity: 1;
    }
}

/* Responsive design */
@media (max-width: 768px) {
    .formDisplay {
        padding: 15px;
    }

    .cardHeader {
        font-size: 1rem;
    }

    .nextBtn {
        font-size: 1rem;
        padding: 0.5rem 1.5rem;
    }
}
