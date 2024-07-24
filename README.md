.sideHead {
  font-size: 14px;
  font-weight: 500;
  margin-top: 0;
  color: #808080;
  position: relative;
  display: inline-block;
  padding-bottom: 5px;
}

.sideHead::after {
  content: '';
  display: block;
  width: 200px;
  height: 2px;
  background: linear-gradient(90deg, #6f36cd 0%, #1f77f6 100%);
  position: absolute;
  bottom: 0;
  left: 0;
}
