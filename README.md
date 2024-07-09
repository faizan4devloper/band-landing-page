.viewAllButton {
  font-weight: 600;
  font-size: 14px;
  color: #808080;
  cursor: pointer;
  transition: background-color 0.3s ease, transform 0.3s ease, color 0.3s ease;
  text-decoration: none;
  display: inline-flex; /* Ensures the icon is inline with the text */
  align-items: center; /* Aligns the text and icon vertically */
  padding: 10px 20px; /* Adds some padding for a better click area */
  border-radius: 5px; /* Slightly rounds the corners */
  box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.1); /* Adds a subtle shadow for a 3D effect */
}

.viewAllButton:hover {
  transform: translateY(-5px); /* Adds a smooth lift on hover */
  text-decoration: underline;
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
