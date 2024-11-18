import React from "react";
import styles from "./WelcomePage.module.css";

const WelcomePage = () => {
  return (
    <div className={styles.container}>
      {/* Header */}
      <header className={styles.header}>
        <div className={styles.logo}>Claim Assist</div>
        <nav className={styles.navbar}>
          <a href="#home">Home</a>
          <a href="#services">Services</a>
          <a href="#faqs">FAQs</a>
          <a href="#contact">Contact Us</a>
        </nav>
        <div className={styles.authButtons}>
          <button className={styles.loginButton}>Login</button>
          <button className={styles.signupButton}>Sign Up</button>
        </div>
      </header>

      {/* Hero Section */}
      <section className={styles.hero}>
        <div className={styles.heroContent}>
          <h1 className={styles.heroTitle}>Simplifying Your Claim Process!</h1>
          <p className={styles.heroSubtitle}>
            Fast, Secure, and Reliable Assistance for All Your Claim Needs.
          </p>
          <button className={styles.ctaButton}>Get Started</button>
        </div>
        <div className={styles.heroImage}></div>
      </section>

      {/* Features Section */}
      <section className={styles.features}>
        <h2 className={styles.sectionTitle}>Why Choose Claim Assist?</h2>
        <div className={styles.featureCards}>
          <div className={styles.card}>
            <div className={styles.icon}>🔄</div>
            <h3>Streamlined Process</h3>
            <p>Experience a smooth and efficient claim process.</p>
          </div>
          <div className={styles.card}>
            <div className={styles.icon}>⏰</div>
            <h3>24/7 Support</h3>
            <p>Our team is always here to help you.</p>
          </div>
          <div className={styles.card}>
            <div className={styles.icon}>🔒</div>
            <h3>Secure & Reliable</h3>
            <p>Data security and reliability you can trust.</p>
          </div>
        </div>
      </section>

      {/* Footer */}
      <footer className={styles.footer}>
        <p>© 2024 Claim Assist. All Rights Reserved.</p>
        <div className={styles.socialLinks}>
          <a href="#facebook">Facebook</a>
          <a href="#twitter">Twitter</a>
          <a href="#linkedin">LinkedIn</a>
        </div>
      </footer>
    </div>
  );
};

export default WelcomePage;




/* Container */
.container {
  font-family: 'Poppins', sans-serif;
  background: linear-gradient(135deg, #f2f2f2 0%, #7ca2e1 100%);
  color: #333;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

/* Header */
.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem 2rem;
  background: #fff;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  position: sticky;
  top: 0;
  z-index: 1000;
}

.logo {
  font-size: 1.5rem;
  font-weight: bold;
  color: #004aad;
}

.navbar a {
  margin: 0 1rem;
  text-decoration: none;
  color: #333;
  font-weight: 500;
}

.authButtons button {
  margin-left: 1rem;
  padding: 0.5rem 1rem;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.loginButton {
  background: transparent;
  color: #004aad;
  border: 2px solid #004aad;
}

.signupButton {
  background: #004aad;
  color: #fff;
}

/* Hero Section */
.hero {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 4rem 2rem;
  background: linear-gradient(135deg, #004aad, #7ca2e1);
  color: white;
}

.heroContent {
  max-width: 50%;
}

.heroTitle {
  font-size: 3rem;
  margin-bottom: 1rem;
}

.heroSubtitle {
  font-size: 1.2rem;
  margin-bottom: 2rem;
}

.ctaButton {
  padding: 1rem 2rem;
  background: #ffd700;
  color: #004aad;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  font-size: 1rem;
}

/* Features Section */
.features {
  padding: 4rem 2rem;
  text-align: center;
}

.sectionTitle {
  font-size: 2rem;
  margin-bottom: 2rem;
}

.featureCards {
  display: flex;
  justify-content: center;
  gap: 2rem;
}

.card {
  background: #fff;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  border-radius: 10px;
  padding: 2rem;
  text-align: center;
  flex: 1;
}

.icon {
  font-size: 2rem;
  margin-bottom: 1rem;
}

/* Footer */
.footer {
  text-align: center;
  padding: 1rem 0;
  background: #004aad;
  color: white;
}

.socialLinks a {
  margin: 0 1rem;
  text-decoration: none;
  color: white;
}
