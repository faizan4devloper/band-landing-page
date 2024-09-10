.card {
  width: 160px;
  overflow: hidden;
  height: 160px;
  border-radius: 12px;
  background-size: cover;
  background-position: center;
  cursor: pointer;
  position: relative;
  margin-bottom: 15px;
  /*margin-top: 15px;*/
  transition: transform 0.6s ease;
  box-shadow: 0px 3px 4px #5c5555;

}

.card:hover{
  transform: translate(0, -10px);
}

.big {
  width: 190px;
  height: 190px;
  z-index: 0;
  transform: scale(1.1);
}

.cardTitle {
  position: absolute;
  font-family: "Poppins", sans-serif;
  font-size: 12px;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 60px; /* Set a fixed height */
  background: linear-gradient(180deg, rgba(0, 0, 0, 0) 0%, #000000 100%),
    linear-gradient(0deg, rgba(23, 23, 25, 0.3), rgba(23, 23, 25, 0.3));
  backdrop-filter: blur(10px); /* Adjust the blur radius as needed */
  border-radius: 12px;
  color: white;
  padding: 18px;
  box-sizing: border-box;
  transition: opacity 0.3s ease;
  text-align: center;
}

.cardContent {
  position: absolute;
  font-family: "Poppins", sans-serif;
  font-size: 10px;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 140px;
  background: linear-gradient(180deg, rgba(0, 0, 0, 0) 0%, #000000 100%),
    linear-gradient(0deg, rgba(23, 23, 25, 0.3), rgba(23, 23, 25, 0.3));
  border-radius: 12px;
    backdrop-filter: blur(10px); /* Adjust the blur radius as needed */
  color: white;
  padding: 18px;
  box-sizing: border-box;
  transform: translateY(100%);
  transition: transform 0.3s ease, background 0.3s ease; /* Adjusted transition timing */
}

.cardContent p{
  backdrop-filter: none;
  font-size: 10px;
}

.cardContent h3{
  color:#eeecec;
}

.cardContent.slide-up {
  transform: translateY(0);
}

.cardContent::before {
  content: "";
  position: absolute;
  padding-top: 0px;
  top: 100%;
  left: 0;
  width: 100%;
  height: 100%;
  background: linear-gradient(to top, #6f36cd, #1f77f6);
  transition: top 0.3s ease;
  border-radius: 12px;
  z-index: -1;
}
.arrow {
 cursor: pointer;
    position: absolute;
    display: flex;
    bottom: 10px;
    right: 10px;
    font-size: 14px;
    width: 16px;
    height: 16px;
    padding: 5px 5px 5px 5px;
    border-radius: 50px;
    border: 2px solid rgba(255, 255, 255, 1);
    color: rgba(255, 255, 255, 1);
    overflow: hidden;
    transition: width 0.3s ease, background 0.5s ease, color 0.5s ease;
}

.arrow:hover {
  width: 80px; /* Increase width on hover */
  align-items: center;
  font-size: 10px;
  background-color: rgba(
    255,
    255,
    255,
    1
  ); /* Change background color on hover */
  color: rgba(15, 95, 220, 1); /* Change text color on hover */
}

.arrow.hovered {
  width: 65px;
}
.card:hover .cardContent::before {
  top: 0;
}
.readMore {
  left: 10px;
}
.card:hover .cardContent::before {
  background: linear-gradient(0deg, #6f36cd 0%, #1f77f6 100%);
}

.big .cardContent {
  transform: translateY(0);
  padding-top:0px;
}


.tagsContainer {
  display: flex;
  flex-wrap: wrap;
  padding: 5px;
  position: absolute;
  top: -15px;
  left: -6px;
  z-index: 10;
  margin-top: 10px;
}

.tag {
  /*border-radius: 3px;*/
  padding: 3px 6px;
  margin-right: 5px;
  font-size: 10px;
  color: #FFF;
  background-color: #6f36cd; 
}

.noDataMessage {
  color: #ff4d4d; /* Red color for emphasis */
  /*font-size: 1em;*/
  font-weight: bold;
  /*margin-top: 10px;*/
  text-align: center;
  padding: 5px;
  border-radius: 5px;
  background-color: #ffe6e6; /* Light red background */
}


/* Styles for the pop-up within the card */
.cardPopup {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background: linear-gradient(180deg, rgba(0, 0, 0, 0) 0%, #000000 100%), linear-gradient(0deg, rgba(23, 23, 25, 0.3), rgba(23, 23, 25, 0.3));
    -webkit-backdrop-filter: blur(10px);
    backdrop-filter: blur(10px);
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3);
  text-align: center;
  width: 80%; /* Adjust width to fit within the card */
  z-index: 10; /* Ensure the pop-up appears above other content */
  animation: fadeIn 0.3s ease-in-out;
  color:#fff;
}
.cardPopup{
  font-size: 12px;
}

/* Close button inside the pop-up */
.closeButton {
background: linear-gradient(90deg, #6f36cd 0%, #1f77f6 100%);
color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 5px;
  cursor: pointer;
  font-size: 1em;
  margin-top: 10px;
}

.closeButton:hover {
  background-color: #0056b3;
}

/* Fade-in animation for the pop-up */
@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translate(-50%, -45%);
  }
  to {
    opacity: 1;
    transform: translate(-50%, -50%);
  }
}
