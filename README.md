    

// reactSelectStyles.js

const customSelectStyles = {
    control: (provided, state) => ({
        ...provided,
        background: 'rgba(255, 255, 255, 0.1)',
        borderColor: state.isFocused ? '#1B98E0' : '#1B98E0',
        boxShadow: state.isFocused ? '0 0 0 1px #FF005C' : 'none',
        '&:hover': {
            borderColor: '#1B98E0',
        },
        color: '#fff',
    }),
    menu: (provided) => ({
        ...provided,
        background: '#fff',
        color: '#000',
        borderRadius: '8px',
        boxShadow: '0 8px 16px rgba(0, 0, 0, 0.2)',
        marginTop: '0.5rem',
    }),
    option: (provided, state) => ({
        ...provided,
        background: state.isSelected
            ? '#2684ff'  // Background color when selected
            : state.isFocused
            ? '#2684ff'  // Background color when focused
            : 'transparent', // Default background color
        color: state.isSelected || state.isFocused ? '#fff' : '#000', // Text color
        cursor: 'pointer',
        margin: '5px 0', // Adds space between menu items
        padding: '8px 12px',
        '&:active': {
            background: '#2684ff',
        },
    }),
    singleValue: (provided) => ({
        ...provided,
        color: '#fff',
    }),
    placeholder: (provided) => ({
        ...provided,
        color: '#e2e2e2',
    }),
    dropdownIndicator: (provided) => ({
        ...provided,
        color: '#fff',
        '&:hover': {
            color: '#FF005C',
        },
    }),
    indicatorSeparator: (provided) => ({
        ...provided,
        background: '#fff',
    }),
};

export default customSelectStyles;


This is my buttons color:-background: linear-gradient(135deg, #F2F2F2 -20%, #7ca2e1);
    This is my whole web app bg color:-background: linear-gradient(135deg, #F2F2F2 0%, #7ca2e1 100%);
match the customSelectStyles according i provided colors
