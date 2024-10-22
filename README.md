.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem 6rem;
  background: linear-gradient(90deg, rgb(95, 30, 193) 0%, rgb(15, 95, 220) 100%);
  border-bottom: 0.1px solid rgb(62, 62, 62);
  transition-duration: 0.5s;
  transition-timing-function: ease-in-out;
  transition-property: transform;
  color: white;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.logoContainer {
  display: flex;
  align-items: center;
  cursor: pointer;
}

.logo {
  width: 100px;
  margin-right: 0.75rem;
}

.citizenAdvisor {
  font-size: 1.8rem;
  font-weight: 700;
  color: white;
  letter-spacing: 0.05rem;
  text-transform: uppercase; /* Optional: makes it more logo-like */
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3); /* Adds depth */
}

.navbar {
  display: flex;
}

.navLinks {
  display: flex;
  list-style: none;
  gap: 1.5rem;
}

.navItem a {
  text-decoration: none;
  color: white;
  font-weight: 500;
  transition: color 0.3s ease;
}

.navItem a:hover {
  color: #e0e7ff; /* Light shade on hover */
}
