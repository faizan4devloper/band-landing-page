.questionBlock {
  margin: 0; /* Remove bottom margin to avoid space between dropdown items */
  border: none; /* Remove border for a smoother look */
  background-color: transparent; /* Make the background transparent to blend in with dropdown */
  transition: background-color 0.3s ease, box-shadow 0.3s ease;
}

.questionBlock:hover {
  background-color: #f0f0f0; /* Slightly darken on hover */
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1); /* Add a subtle shadow on hover */
}

.question {
  font-size: 1rem; /* Maintain same font size */
  font-weight: bold; /* Bold for emphasis */
  padding: 10px 14px; /* Adjust padding for a comfortable click area */
  cursor: pointer; /* Pointer cursor to indicate interactivity */
  color: #2a2a2a; /* Dark text color for visibility */
  display: flex; /* Flexbox for alignment */
  justify-content: space-between; /* Space between question text and chevron */
  align-items: center; /* Center items vertically */
  transition: color 0.3s ease; /* Smooth color transition */
}

.question:hover {
  color: #0073e6; /* Change color on hover */
}

.chevronIcon {
  font-size: 1rem; /* Maintain size */
  color: #888; /* Icon color */
  margin-left: 10px; /* Space from question text */
  transition: transform 0.3s ease, color 0.3s ease; /* Smooth transitions for icon */
}

/* Add additional styling for the expanded answer if needed */
.gridAnswer {
  padding: 10px; /* Padding around the answer content */
  background-color: #fafafa; /* Light background for answers */
  border-top: 1px solid #ddd; /* Subtle border to separate question and answer */
}
