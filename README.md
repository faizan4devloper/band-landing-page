.sideHead {
  font-size: 18px;
  font-weight: 500;
  margin-top: 0;
  margin-left: 15px;
  color: #6f36cd; /* Adjust the color as desired */
  position: relative;
  display: inline-block;
  padding-bottom: 5px;
}

.sideHead::after {
  content: '';
  display: block;
  width: 50%;
  height: 2px;
  background: linear-gradient(90deg, #6f36cd 0%, #1f77f6 100%);
  position: absolute;
  bottom: 0;
  left: 0;
}
