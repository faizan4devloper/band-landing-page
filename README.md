.footer {
  background: linear-gradient(to bottom, #14142b 0%, #05041e 100%) !important;
  padding: 20px;
  text-align: center;
  color: #fcfcfc;
  border-top: 1px solid #ddd;
  margin: 30px -88px;
  margin-bottom: 0px;
  height: 100%;
  position: relative;
  overflow: hidden; /* Ensures that animations don’t overflow */
}

.blogSection {
  margin-bottom: 15px;
}

.blogTitle {
  font-size: 24px;
  color: #5931d5;
  margin-bottom: 10px;
  transition: color 0.3s ease;
}

.blogTitle:hover {
  color: #fcfcfc; /* Change title color on hover */
}

.blogList {
  list-style: none;
  padding: 0;
  animation: fadeIn 1s ease-out; /* Animation for list items */
}

.blogLink {
  color: #fcfcfc;
  text-decoration: none;
  transition: color 0.3s ease, transform 0.3s ease; /* Added transform transition */
  display: inline-block;
}

.blogLink:hover {
  color: #5931d5;
  transform: translateY(-3px); /* Lift effect on hover */
}

.footerInfo {
  margin-top: 20px;
  font-size: 14px;
  color: #888;
}

.footerInfo p {
  margin: 5px 0;
  transition: color 0.3s ease;
}

.footerInfo p:hover {
  color: #5931d5; /* Highlight contact info on hover */
}

/* Keyframes for fading in blog list */
@keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}
