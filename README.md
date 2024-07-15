.indicatorsContainer {
  display: flex;
  justify-content: center;
  list-style: none;
  padding: 0;
  margin-top: 10px;
  position: absolute; /* Position indicators at the bottom */
  bottom: 10px;
  width: 100%;
  z-index: 2; /* Ensure indicators appear above the carousel items */
}

.dot {
  width: 12px;
  height: 12px;
  border-radius: 50%;
  display: inline-block;
  margin: 0 5px;
  cursor: pointer;
  background-color: #ccc;
  transition: transform 0.3s ease, background-color 0.3s ease; /* Smooth transition for color and scale */
}

.dot.selected {
  background-color: #6f36cd; /* Purple color for selected dot */
  transform: scale(1.5); /* Scale up selected dot */
}
