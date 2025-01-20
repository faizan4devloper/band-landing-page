import React from 'react';
import { motion } from 'framer-motion';
import './LandingPage.css';

// Animated Background Component
const AnimatedBackground = () => {
  const bubbles = Array(10).fill(0).map((_, index) => (
    <motion.div
      key={index}
      className="bubble"
      style={{
        width: `${Math.random() * 100 + 50}px`,
        height: `${Math.random() * 100 + 50}px`,
        left: `${Math.random() * 100}%`,
        top: `${Math.random() * 100}%`,
        opacity: Math.random() * 0.5 + 0.2
      }}
      animate={{
        x: [0, Math.random() * 100 - 50, 0],
        y: [0, Math.random() * 100 - 50, 0],
        rotate: 360
      }}
      transition={{
        duration: Math.random() * 5 + 5,
        repeat: Infinity,
        repeatType: "reverse",
        ease: "easeInOut"
      }}
    />
  ));

  return <div className="background-bubbles">{bubbles}</div>;
};

const LandingPage = ({ onGetStarted }) => {
  const pageVariants = {
    initial: { opacity: 0, scale: 0.9 },
    in: { opacity: 1, scale: 1 },
    out: { opacity: 0, scale: 1.1 }
  };

  const pageTransition = {
    type: "tween",
    ease: "anticipate",
    duration: 0.5
  };

  return (
    <motion.div 
      className="landing-page"
      initial="initial"
      animate="in"
      exit="out"
      variants={pageVariants}
      transition={pageTransition}
    >
      <div className="landing-content">
        <motion.h1
          initial={{ y: -50, opacity: 0 }}
          animate={{ y: 0, opacity: 1 }}
          transition={{ delay: 0.2 }}
        >
          Welcome to AI Assistant
        </motion.h1>
        
        <motion.p
          initial={{ y: 50, opacity: 0 }}
          animate={{ y: 0, opacity: 1 }}
          transition={{ delay: 0.4 }}
        >
          Your Intelligent Conversational Companion
        </motion.p>
        
        <motion.button
          className="get-started-btn"
          whileHover={{ 
            scale: 1.05,
            boxShadow: "0px 0px 20px rgba(255,255,255,0.5)"
          }}
          whileTap={{ scale: 0.95 }}
          onClick={onGetStarted}
          initial={{ opacity: 0, y: 20 }}
          animate={{ opacity: 1, y: 0 }}
          transition={{ delay: 0.6 }}
        >
          Get Started
        </motion.button>
      </div>

      {/* Animated Background Elements */}
      <AnimatedBackground />
    </motion.div>
  );
};

export default LandingPage;



:root {
  --primary-color: #6a11cb;
  --secondary-color: #2575fc;
  --text-color: #ffffff;
  --background-gradient: linear-gradient(135deg, #6a11cb 0%, #2575fc 100%);
}

.landing-page {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 100vh;
  position: relative;
  background-image: 
    linear-gradient(rgba(0, 0, 0, 0.5), rgba(0, 0, 0, 0.5)), /* Opacity gradient overlay */
    url('./LandingImg.jpg');
  background-size: cover;
  background-position: center;
  overflow: hidden;
}


.landing-content {
  position: relative;
  z-index: 10;
  text-align: center;
  max-width: 800px;
  padding: 2rem;
}

.landing-content h1 {
  font-size: 3.5rem;
  font-weight: 800;
  margin-bottom: 1rem;
  background: linear-gradient(to right, #ffffff, #f0f0f0);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  text-shadow: 0 4px 15px rgba(0,0,0,0.2);
}

.landing-content p {
  font-size: 1.25rem;
  font-weight: 300;
  margin-bottom: 2rem;
  color: rgba(255,255,255,0.8);
  line-height: 1.6;
}

.get-started-btn {
  position: relative;
  padding: 12px 30px;
  font-size: 1rem;
  font-weight: 600;
  color: var(--primary-color);
  background-color: #ffffff;
  border: none;
  border-radius: 50px;
  cursor: pointer;
  overflow: hidden;
  transition: all 0.3s ease;
  box-shadow: 0 10px 25px rgba(0,0,0,0.1);
}

.get-started-btn::before {
  content: '';
  position: absolute;
  top: 0;
  left: -100%;
  width: 100%;
  height: 100%;
  background: linear-gradient(
    120deg, 
    transparent, 
    rgba(255,255,255,0.3), 
    transparent
  );
  transition: all 0.5s ease;
}

.get-started-btn:hover::before {
  left: 100%;
}

.get-started-btn:hover {
  transform: translateY(-5px);
  box-shadow: 0 15px 35px rgba(0,0,0,0.2);
}

.background-bubbles {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 1;
  overflow: hidden;
}

.bubble {
  position: absolute;
  background: rgba(255,255,255,0.1);
  border-radius: 50%;
  backdrop-filter: blur(10px);
}
