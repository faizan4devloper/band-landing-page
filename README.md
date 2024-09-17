/* Webkit Browsers */
::-webkit-scrollbar {
    width: 12px;
}

::-webkit-scrollbar-track {
    background: linear-gradient(180deg, #e0e0e0, #f1f1f1); /* Gradient background */
    border-radius: 10px;
}

::-webkit-scrollbar-thumb {
    background: linear-gradient(180deg, #888, #555); /* Gradient handle */
    border-radius: 10px;
    box-shadow: inset 0 0 5px rgba(0, 0, 0, 0.2); /* Inner shadow */
}

::-webkit-scrollbar-thumb:hover {
    background: linear-gradient(180deg, #555, #333); /* Darker gradient on hover */
}

/* Firefox */
body {
    scrollbar-width: thin;
    scrollbar-color: #888 #f1f1f1;
}
