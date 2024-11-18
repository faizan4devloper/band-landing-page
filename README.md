import React, { useEffect, useRef } from "react";
import { gsap } from "gsap";
import styles from "./WelcomePage.module.css";

const WelcomePage = () => {
  const heroContentRef = useRef(null);
  const heroImageRef = useRef(null);
  const featureCardsRef = useRef([]);

  useEffect(() => {
    // Hero Section Animation
    gsap.fromTo(
      heroContentRef.current,
      { opacity: 0, x: -50 },
      { opacity: 1, x: 0, duration: 1, ease: "power3.out" }
    );

    gsap.fromTo(
      heroImageRef.current,
      { opacity: 0, x: 50 },
      { opacity: 1, x: 0, duration: 1, ease: "power3.out", delay: 0.3 }
    );

    // Feature Cards Animation
    gsap.fromTo(
      featureCardsRef.current,
      { opacity: 0, y: 50 },
      {
        opacity: 1,
        y: 0,
        duration: 0.6,
        ease: "power3.out",
        stagger: 0.2, // Delay between animations
      }
    );
  }, []);

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
        <div ref={heroContentRef} className={styles.heroContent}>
          <h1 className={styles.heroTitle}>Simplifying Your Claim Process!</h1>
          <p className={styles.heroSubtitle}>
            Fast, Secure, and Reliable Assistance for All Your Claim Needs.
          </p>
          <button className={styles.ctaButton}>Get Started</button>
        </div>
        <div ref={heroImageRef} className={styles.heroImage}></div>
      </section>

      {/* Features Section */}
      <section className={styles.features}>
        <h2 className={styles.sectionTitle}>Why Choose Claim Assist?</h2>
        <div className={styles.featureCards}>
          {["🔄", "⏰", "🔒"].map((icon, index) => (
            <div
              key={index}
              ref={(el) => (featureCardsRef.current[index] = el)}
              className={styles.card}
            >
              <div className={styles.icon}>{icon}</div>
              <h3>
                {index === 0
                  ? "Streamlined Process"
                  : index === 1
                  ? "24/7 Support"
                  : "Secure & Reliable"}
              </h3>
              <p>
                {index === 0
                  ? "Experience a smooth and efficient claim process."
                  : index === 1
                  ? "Our team is always here to help you."
                  : "Data security and reliability you can trust."}
              </p>
            </div>
          ))}
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





/* For smooth animations, ensure elements are initially hidden */
.heroContent, .heroImage, .card {
  opacity: 0; /* Starting state for GSAP animations */
}
