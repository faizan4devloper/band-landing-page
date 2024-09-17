.breadcrumbs {
    display: flex;
    align-items: center;
    margin: 1rem;
    font-size: 1rem;
    color: #333;
}

.crumb {
    display: flex;
    align-items: center;
}

.icon {
    margin-right: 0.5rem;
    color: #007bff;
}

.link {
    color: #007bff; /* Link color */
    text-decoration: none;
    transition: color 0.3s ease;
}

.link:hover {
    color: #0056b3; /* Darker shade on hover */
    text-decoration: underline;
}

.current {
    font-weight: bold;
    color: #555; /* Current breadcrumb color */
}

.separator {
    margin: 0 0.5rem;
    color: #999;
    font-size: 0.8rem; /* Slightly smaller size for separators */
}
