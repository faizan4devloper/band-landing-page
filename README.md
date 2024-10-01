/* Main container for the form display */
.formDisplay {
    background-color: #1e293b; /* Same dark background as sidebar */
    padding: 20px;
    border-radius: 12px;
    box-shadow: 0 12px 30px rgba(0, 0, 0, 0.4); /* Same strong shadow as sidebar */
    flex: 3; /* Flex-grow to take up space proportionally */
    transition: all 0.3s ease; /* Smooth transitions for any future changes */
    max-height: 100%; /* Maintain a fixed height */
    display: flex;
    justify-content: center;
    align-items: center;
    color: #e2e8f0; /* Light text color like the sidebar */
    overflow: auto; /* Handle overflow in case content exceeds container height */
}

/* Style the forms inside the formDisplay */
.formDisplay > * {
    margin-bottom: 20px;
    width: 100%; /* Ensure forms take full width */
}

/* Text when no form is selected */
.formDisplay p {
    color: #94a3b8; /* Muted blue-grey text color */
    text-align: center;
    font-size: 1.2rem;
    padding: 40px 0;
}

/* Responsive behavior for smaller screens */
@media (max-width: 768px) {
    .formDisplay {
        padding: 15px;
        max-width: 100%; /* Adjust width for mobile */
        flex: none; /* Override flex-grow behavior for small screens */
    }

    .formDisplay p {
        font-size: 1rem;
    }
}
