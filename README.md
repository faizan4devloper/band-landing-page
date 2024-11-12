/* Wrapper for the image */
.imageWrapper {
  position: relative;
  display: inline-block;
  cursor: pointer;
}

/* Tooltip styles */
.tooltip {
  visibility: hidden;
  position: absolute;
  bottom: 100%; /* Position the tooltip above the image */
  left: 50%;
  transform: translateX(-50%);
  background-color: rgba(0, 0, 0, 0.75);
  color: #fff;
  text-align: center;
  border-radius: 5px;
  padding: 5px;
  font-size: 0.9rem;
  opacity: 0;
  transition: opacity 0.3s, visibility 0s 0.3s; /* Smooth fade-in/out */
  z-index: 1;
}

/* Show tooltip on hover */
.imageWrapper:hover .tooltip {
  visibility: visible;
  opacity: 1;
  transition: opacity 0.3s, visibility 0s;
}

/* Image styles */
.responseImage {
  max-width: 100%;
  height: auto;
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
}

/* You can also add hover effects for the image if needed */
.responseImage:hover {
  opacity: 0.8;
  transition: opacity 0.3s ease;
}
