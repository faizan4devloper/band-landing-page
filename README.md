import React from "react";
import Header from "./Header";
import HeroSection from "./HeroSection";
import FeaturesSection from "./FeaturesSection";
import Footer from "./Footer";
import styles from "./WelcomePage.module.css";

const WelcomePage = () => {
  return (
    <div className={styles.container}>
      <Header />
      <HeroSection />
      <FeaturesSection />
      <Footer />
    </div>
  );
};

export default WelcomePage;



import React from "react";
import styles from "./Header.module.css";

const Header = () => {
  return (
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
  );
};

export default Header;



import React, { useEffect, useRef } from "react";
import { gsap } from "gsap";
import styles from "./HeroSection.module.css";

const HeroSection = () => {
  const heroContentRef = useRef(null);
  const heroImageRef = useRef(null);

  useEffect(() => {
    gsap.fromTo(
      heroContentRef.current,
      { opacity: 0, y: -50 },
      { opacity: 1, y: 0, duration: 1.5, ease: "power3.out", delay: 0.5 }
    );

    gsap.fromTo(
      heroImageRef.current,
      { opacity: 0, scale: 0.9 },
      { opacity: 1, scale: 1, duration: 1.5, ease: "power3.out", delay: 1 }
    );
  }, []);

  return (
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
  );
};

export default HeroSection;



import React, { forwardRef } from "react";
import styles from "./FeatureCard.module.css";

const FeatureCard = forwardRef(({ icon, title, description }, ref) => {
  return (
    <div ref={ref} className={styles.card}>
      <div className={styles.icon}>{icon}</div>
      <h3>{title}</h3>
      <p>{description}</p>
    </div>
  );
});

export default FeatureCard;
