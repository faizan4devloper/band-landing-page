import React, { useEffect, useRef } from "react";
import { gsap } from "gsap";
import { ScrollTrigger } from "gsap/ScrollTrigger";
import styles from "./WelcomePage.module.css";

gsap.registerPlugin(ScrollTrigger);

const WelcomePage = () => {
  const heroContentRef = useRef(null);
  const heroImageRef = useRef(null);
  const featureCardsRef = useRef([]);
  const footerRef = useRef(null);

  useEffect(() => {
    // Hero Section Animation
    gsap.fromTo(
      heroContentRef.current,
      { opacity: 0, y: -50 },
      {
        opacity: 1,
        y: 0,
        duration: 1.5,
        ease: "power3.out",
        delay: 0.5,
      }
    );

    gsap.fromTo(
      heroImageRef.current,
      { opacity: 0, scale: 0.9 },
      {
        opacity: 1,
        scale: 1,
        duration: 1.5,
        ease: "power3.out",
        delay: 1,
      }
    );

    // Feature Cards Scroll Animation
    featureCardsRef.current.forEach((card, index) => {
      gsap.fromTo(
        card,
        { opacity: 0, y: 50 },
        {
          opacity: 1,
          y: 0,
          duration: 1,
          ease: "power3.out",
          scrollTrigger: {
            trigger: card,
            start: "top 90%",
            end: "top 60%",
            toggleActions: "play none none reverse",
          },
        }
      );
    });

    // Footer Fade-in Effect
    gsap.fromTo(
      footerRef.current,
      { opacity: 0, y: 50 },
      {
        opacity: 1,
        y: 0,
        duration: 1.5,
        ease: "power3.out",
        scrollTrigger: {
          trigger: footerRef.current,
          start: "top 95%",
          toggleActions: "play none none reverse",
        },
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
      <footer ref={footerRef} className={styles.footer}>
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



/* General Styling */
.container {
  font-family: 'Poppins', sans-serif;
  background: linear-gradient(135deg, #f2f2f2 0%, #7ca2e1 100%);
  color: #333;
  min-height: 100vh;
  overflow-x: hidden;
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

/* Hero Section */
.hero {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 4rem 2rem;
  background: linear-gradient(135deg, #004aad, #7ca2e1);
  color: white;
  min-height: 70vh;
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
  transition: transform 0.3s ease;
}

.ctaButton:hover {
  transform: scale(1.1);
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
  transform: translateY(10px);
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.card:hover {
  transform: translateY(-10px);
  box-shadow: 0 8px 16px rgba(0, 0, 0, 0.2);
}

/* Footer */
.footer {
  text-align: center;
  padding: 1rem 0;
  background: #004aad;
  color: white;
}
