/* Styled scrollbar for modal content */
.modalContent {
  position: relative;
  background: white;
  padding: 20px 20px 30px;
  border-radius: 10px;
  text-align: center;
  width: 600px;
  max-width: 90%;
  max-height: 90vh;
  overflow-y: auto;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
  display: flex;
  flex-direction: column;
  align-items: center;

  /* Custom scrollbar styles for WebKit browsers */
  scrollbar-width: thin; /* Firefox */
  scrollbar-color: #572ac2 #f4f4f4; /* Firefox */
}

/* WebKit scrollbar styling */
.modalContent::-webkit-scrollbar {
  width: 8px; /* Set the width of the scrollbar */
}

.modalContent::-webkit-scrollbar-track {
  background: #f4f4f4; /* Track color */
  border-radius: 10px;
}

.modalContent::-webkit-scrollbar-thumb {
  background-color: #572ac2; /* Thumb color */
  border-radius: 10px;
  border: 2px solid #f4f4f4; /* Adds space around thumb */
}

.modalContent::-webkit-scrollbar-thumb:hover {
  background-color: #4080f5; /* Thumb color on hover */
}
