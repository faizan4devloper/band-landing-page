/* Custom Indicator Styles */
.indicatorsContainer {
  display: flex;
  justify-content: center;
  list-style: none;
  padding: 0;
  margin-top: 10px;
}

.dot {
  width: 12px;
  height: 12px;
  border-radius: 50%;
  display: inline-block;
  margin: 0 5px;
  cursor: pointer;
  transition: background-color 0.3s ease, box-shadow 0.3s ease; /* Added box-shadow transition */
  background-color: #f58529; /* Suitable orange color */
  box-shadow: 0 0 6px rgba(245, 133, 41, 0.8); /* Orange box shadow */
}

.dot.selected {
  background-color: #6f36cd; /* Purple color for selected dot */
  box-shadow: 0 0 12px rgba(111, 54, 205, 0.8); /* Purple box shadow */
}
