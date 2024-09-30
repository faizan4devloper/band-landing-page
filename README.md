/* Main container for the form display */
.formDisplay {
    background-color: #f8fafc; /* Light background for contrast */
    padding: 20px;
    border-radius: 12px;
    box-shadow: 0 8px 24px rgba(0, 0, 0, 0.1); /* Soft shadow for depth */
    max-width: 1000px; /* Set a maximum width for better readability */
    margin: 20px auto; /* Center the form display */
    transition: all 0.3s ease; /* Smooth transitions for any future changes */
}

/* Style the forms inside the formDisplay */
.formDisplay > * {
    margin-bottom: 20px; /* Add space between form elements */
}

/* When no form is selected */
.formDisplay p {
    color: #64748b; /* Muted blue-grey for the placeholder text */
    text-align: center;
    font-size: 1.2rem;
    padding: 40px 0;
}

/* Responsive behavior for smaller screens */
@media (max-width: 768px) {
    .formDisplay {
        padding: 15px;
        max-width: 90%; /* Allow the form to scale down on smaller screens */
    }

    .formDisplay p {
        font-size: 1rem; /* Adjust font size for readability on small devices */
    }
}
