
const customStyles = {
  control: (provided, state) => ({
    ...provided,
    borderRadius: "4px",
    width: "94%", // Control width
    border: state.isFocused ? "1px solid #5f1ec1" : "1px solid #ccc",
    cursor: "pointer",
    backgroundColor: "#fff",
    color: "#555",
  }),
  option: (provided, state) => ({
    ...provided,
    backgroundColor: state.isSelected ? "#5f1ec1" : "#fff",
    color: state.isSelected ? "#fff" : "#555",
    fontSize: "12px", // Option font size
    width: "92%", // Option width
    cursor: "pointer",
  }),
  placeholder: (provided) => ({
    ...provided,
    fontSize: "12px", // Placeholder font size
  }),
};
