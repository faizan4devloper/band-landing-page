import React, { useRef, useEffect } from "react";
import { gsap } from "gsap";
import { ScrollTrigger } from "gsap/ScrollTrigger";
import FeatureCard from "./FeatureCard";
import styles from "./FeaturesSection.module.css";

gsap.registerPlugin(ScrollTrigger);

const FeaturesSection = () => {
  const featureCardsRef = useRef([]);

  useEffect(() => {
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
  }, []);

  const features = [
    {
      icon: "🔄",
      title: "Streamlined Process",
      description: "Experience a smooth and efficient claim process.",
    },
    {
      icon: "⏰",
      title: "24/7 Support",
      description: "Our team is always here to help you.",
    },
    {
      icon: "🔒",
      title: "Secure & Reliable",
      description: "Data security and reliability you can trust.",
    },
  ];

  return (
    <section className={styles.features}>
      <h2 className={styles.sectionTitle}>Why Choose Claim Assist?</h2>
      <div className={styles.featureCards}>
        {features.map((feature, index) => (
          <FeatureCard
            key={index}
            ref={(el) => (featureCardsRef.current[index] = el)}
            icon={feature.icon}
            title={feature.title}
            description={feature.description}
          />
        ))}
      </div>
    </section>
  );
};

export default FeaturesSection;
