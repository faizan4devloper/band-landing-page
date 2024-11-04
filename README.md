.responseSection {
  padding: 15px;
  font-size: .8rem;
  background-color: #f9f9f9;
  border-left: 4px solid #0073e6;
  border-radius: 6px;
  position: relative;
  overflow: hidden;
  height: 80px; /* Fixed height for collapsed state */
  transition: height 0.4s ease;
  width: 100%;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.responseSection.expanded {
  height: auto; /* Allow section to expand fully */
  overflow-y: auto; /* Enable scroll within expanded section */
}

.responseSection.expanded::-webkit-scrollbar {
  width: 6px;
}

.responseSection.expanded::-webkit-scrollbar-thumb {
  background-color: rgba(15, 95, 220, 1);
  border-radius: 10px;
}
