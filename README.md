.footer {
  background: linear-gradient(to bottom, #1a1a2e, #16213e);
  border-top: 0.1px solid rgba(219, 197, 255, 1);
  padding: 40px 20px;
  text-align: center;
  color: #fcfcfc;
  margin: 30px -88px;
  margin-top: 100px;
  margin-bottom: 0px;
  position: relative;
  overflow: hidden;
}

.footerHeading {
  font-size: 28px;
  margin-bottom: 40px;
  color: #fff;
  margin-top: 0px;
  letter-spacing: 2px;
  /*border-bottom: 0.1px solid #fff;*/
  padding-bottom: 10px;
}

.cardContainer {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 30px;
}

.card {
  background: linear-gradient(90deg, #6f36cd 0%, #1f77f6 100%);
  border-radius: 12px;
  padding: 20px 15px;
  width: 240px; /* Increased width for better content fit */
  color: #fcfcfc;
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.2);
  transition: transform 0.3s ease, box-shadow 0.3s ease;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  text-align: left;
  position: relative;
}

.card:hover {
  transform: translateY(-10px);
  box-shadow: 0 20px 30px rgba(0, 0, 0, 0.3);
}

.iconContainer {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-bottom: 15px;
}

.cardIcon {
  font-size: 40px; /* Larger icon size */
  color: #fff;
}

.cardTitle {
  font-size: 18px; /* Increased font size */
  font-weight: bold; /* Bold for emphasis */
  margin-bottom: 10px;
  color: #fff;
  line-height: 1.4;
}

.cardDescription {
  font-size: 13px; /* Increased font size for better readability */
  font-weight: 500; /* Semi-bold for better emphasis */
  color: #f0f0f0;
  margin-top: 0px;
  margin-bottom: 20px;
}

.cardLink {
  margin-top: auto;
  color: #fff;
  text-decoration: none;
  font-weight: bold;
  display: inline-flex;
  align-items: center;
  gap: 5px;
  font-size: 16px;
  overflow: hidden; /* Hide overflow for smooth transition */
  transition: color 0.3s ease;
  position: relative; /* Position relative for controlling internal movements */
}

.arrowOnly {
  display: inline-block;
  transition: transform 0.3s ease; /* Smooth transition for moving the arrow */
  transform: translateX(0); /* Initial position of the arrow */
}

.linkText {
  display: inline-block;
  opacity: 0; /* Initially hide the text */
  margin-right: -110px; /* Start off-screen to the left */
  transition: opacity 0.3s ease, margin-right 0.3s ease; /* Transition effects */
}

.cardLink:hover .arrowOnly {
  transform: translateX(10px); /* Move arrow to the right on hover */
}

.cardLink:hover .linkText {
  opacity: 1; /* Make text visible */
  margin-right: 1px; /* Slide text into view */
}

/* Hover effect remains unchanged */
.cardLink:hover {
  color: #ff80bf;
}

.footerInfo {
  margin-top: 30px;
  font-size: 14px;
  color: #888;
}

.footerInfo p {
  margin: 5px 0;
  transition: color 0.3s ease;
}

.footerInfo p:hover {
  color: #ff80bf;
}
