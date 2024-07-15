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
}

.cardsContainer {
  gap: 20px;
  border-radius: 12px;
  display: flex;
  justify-content: center;
}

.arrow {
  cursor: pointer;
  position: relative;
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
  left: -25px;
  top: 90px;
}

.rightArrow {
  right: -25px;
  top: 90px;
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
  /*transition: opacity 0.1s, visibility 0.1s;*/
}

.logo img:hover::after {
  opacity: 1;
  visibility: visible;
}

.viewAllContainer {
  width: 100%;
  display: flex;
  justify-content: right;
  align-items: center;
  /*margin-bottom: 20px;*/
  margin-right: 112px;
}

.solutionHead{
    font-weight: 600;
    font-size: 14px;
    color: #808080;
      margin-left: 118px;

}



.viewAllButton {
  font-weight: 600;
  font-size: 14px;
  color: #808080;
  cursor: pointer;
  transition: background-color 0.3s ease, transform 0.3s ease, color 0.3s ease;
  text-decoration: none;
  display: inline-flex; 
  align-items: center; 
  padding: 6px 7px; 
  border-radius: 5px; 
  box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.1); 
}

.viewAllButton:hover {
  transform: translateY(-5px); /* Adds a smooth lift on hover */
  color: #5f1ec1;
  background-color: rgba(13, 85, 198, 0.1); /* Adds a subtle background color change on hover */
  box-shadow: 0px 8px 15px rgba(0, 0, 0, 0.2); /* Enhances the shadow effect on hover */
}

.icon {
  margin-left: 8px; /* Adds space between the text and the icon */
  transition: transform 0.3s ease; /* Smooth transition for the icon */
}

.viewAllButton:hover .icon {
  transform: translateX(5px); /* Moves the icon slightly to the right on hover */
}


.cardsContainer {
  gap: 20px;
  border-radius: 12px;
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
}

.scrollDownButton {
  position: fixed;
  left: 20px; /* Adjust as per your layout */
  bottom: 20px; /* Adjust as per your layout */
  background: linear-gradient(90deg, #6f36cd 0%, #1f77f6 100%);
  color: white;
  border: none;
  cursor: pointer;
  border-radius: 4px;
  width: 21px;
  height: 22px;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 12px;
  cursor: pointer;
  transition: background-color 0.3s ease;
    transition: .4s ease

}

.scrollDownButton:hover {
  background-color: rgba(13, 85, 198, 1); /* Darker shade on hover */
}
.scrollUpButton {
  position: fixed;
  left: 20px; /* Adjust as per your layout */
  bottom: 20px; /* Adjust as per your layout */
  background: linear-gradient(90deg, #6f36cd 0%, #1f77f6 100%);
  color: white;
  border: none;
  cursor: pointer;
  border-radius: 4px;
  width: 21px;
  height: 22px;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 12px;
  cursor: pointer;
  transition: background-color 0.3s ease;
  transition: .4s ease
}

.scrollUpButton:hover {
  background-color: rgba(13, 85, 198, 1); /* Darker shade on hover */
}
