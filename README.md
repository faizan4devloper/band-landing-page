.mainContent {
  flex-grow: 1;
  height: 100vh;
  padding: 40px; /* Increased padding for better spacing */
  padding-top: 60px;
  background-color: #f7f9fc; /* Light background color */
  overflow-y: auto; /* Allow scrolling if content exceeds the viewport */
  width: 800px; /* Set a fixed width */
  max-width: 100%; /* Ensure it doesn't overflow on smaller screens */
  border-radius: 10px; /* Rounded corners */
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1); /* More pronounced shadow */
  display: flex;
  flex-direction: column; /* Stack children vertically */
  gap: 20px; /* Space between children */
}

.loaderWrapper {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 60vh; /* Center the loader in the middle of the viewport */
  color: #0073e6; /* Loader color to match theme */
  font-size: 1.5rem; /* Increased font size for better visibility */
}




.questionBlock {
  padding: 20px; /* Increased padding for comfort */
  margin: 20px 0; /* Top and bottom margin for spacing from other elements */
  border: 1px solid #ddd; /* Light border for separation */
  border-radius: 5px; /* Rounded corners */
  background-color: #ffffff; /* White background for clarity */
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1); /* Subtle shadow */
  transition: transform 0.2s; /* Smooth scaling effect on hover */
}

.questionBlock:hover {
  transform: scale(1.02); /* Slightly scale up on hover */
}

.question {
  font-size: 1.5rem; /* Larger font size for question text */
  font-weight: bold; /* Bold for emphasis */
  color: #2a2a2a; /* Dark text color for readability */
  margin-bottom: 10px; /* Space below question text */
}

.answer {
  font-size: 1.1rem; /* Normal font size for answers */
  color: #444; /* Dark grey for answers */
  margin-top: 10px; /* Space above answer text */
  padding: 10px; /* Padding around answer */
  background-color: #fafafa; /* Light background for answers */
  border-left: 4px solid #0073e6; /* Blue left border for visual interest */
  border-radius: 4px; /* Slight rounding for the answer block */
}
