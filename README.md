.sideBarPage {
  /*display: flex;*/
  position: fixed;
  margin-top: 70px;
  margin-left: 45px;
  /* margin-top: 25px; */
  flex-direction: column;
  min-height: 100vh;
  overflow-y: auto; /* Enable vertical scrolling */
  overscroll-behavior: contain; /* Prevent overscrolling */
  scroll-behavior: smooth; /* Enable smooth scrolling */
  
}

.header2 {
  display: flex;
  align-items: center;
}
.cardTitle {
  font-size: 18px;
  color: rgba(23, 23, 25, 1);
  font-weight: 600;
  margin-bottom: 10px;
  font-family: "Poppins", sans-serif;
}
.contentWrapper {
  display: flex;
  /*margin-top: 15px;*/
  flex: 1;
}
.backButtonContainer {
  display: flex;
  align-items: center;
  padding: 10px;
}

.backButton {
  background-color: rgba(230, 235, 245, 1);
  padding: 7px;
  margin-left: 20px;
  margin-bottom: 10px;
  border-radius: 4px;
  width: 32px;
  font-size: 14px;
  border: none;
  cursor: pointer;
  margin-right: 10px;
}

.backButton:hover {
  color: rgba(95, 30, 193, 1); /* Change button color on hover */
}
