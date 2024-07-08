
@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600;700&display=swap");

/* Hide the scrollbar */
::-webkit-scrollbar {
  width: 0; /* Remove scrollbar width */
  height: 0; /* Remove scrollbar height */
}

html,
body {
  overflow-y: scroll; /* Always show vertical scrollbar */
  font-family: "Poppins", sans-serif;
}

.app {
  width: 1100px;
  height: 600px;
  margin: 0 auto;
  position: relative;
}

.cardsContainer {
  gap: 20px;
  border-radius: 12px;
  display: flex;
  justify-content: center;
  overflow: hidden; /* Ensure no horizontal scroll */
}

.card {
  flex: 1 0 200px; /* Flex item with fixed width */
  margin: 10px; /* Margin to create space between cards */
}

.arrow {
  cursor: pointer;
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  font-size: 18px;
  width: 17px;
  height: 22px;
  padding: 3px 8px 5px 6px;
  border-radius: 50px;
  border: 2px solid rgba(15, 95, 220, 1);
  color: rgba(15, 95, 220, 1); /* Adjust color as needed */
  transition: transform 0.5s ease, background 0.5s ease; /* Add transition */
}

.arrow:hover {
  background-color: rgba(15, 95, 220, 1); /* Change background color on hover */
  color: white; /* Change text color on hover */
}

.leftArrow {
  left: 10px; /* Adjust position to be within container */
}

.rightArrow {
  right: 10px; /* Adjust position to be within container */
}

@media screen and (max-width: 1100px) {
  .app {
    padding: 0 10px;
  }
}

@media screen and (max-width: 768px) {
  .app {
    max-width: 100%;
  }
}

/* Tooltip styling */
.arrow img::after {
  content: attr(title);
  position: absolute;
  top: 100%;
  left: 50%;
  transform: translateX(-50%);
  background-color: #333;
  color: #fff;
  padding: 4px;
  border-radius: 5px;
  font-size: 8px;
  white-space: nowrap;
  opacity: 0;
  visibility: hidden;
  transition: opacity 0.1s, visibility 0.1s;
}

.arrow img:hover::after {
  opacity: 1;
  visibility: visible;
}
