<!-- index.html -->
<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600;800&family=Roboto:wght@300;400;700&family=Montserrat:wght@300;400;700&display=swap" rel="stylesheet">



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
          style={{ fontFamily: 'Poppins, sans-serif' }}
        >
          Welcome to AI Assistant
        </motion.h1>
        
        <motion.p
          initial={{ y: 50, opacity: 0 }}
          animate={{ y: 0, opacity: 1 }}
          transition={{ delay: 0.4 }}
          style={{ fontFamily: 'Roboto, sans-serif' }}
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



.landing-content h1 {
  font-size: 3.5rem;
  font-weight: 800;
  margin-bottom: 1rem;
  background: linear-gradient(to right, #ffffff, #f0f0f0);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  text-shadow: 0 4px 15px rgba(0,0,0,0.2);
  font-family: 'Poppins', sans-serif; /* Example modern font */
}

.landing-content p {
  font-size: 1.25rem;
  font-weight: 300;
  margin-bottom: 2rem;
  color: rgba(255,255,255,0.8);
  line-height: 1.6;
  font-family: 'Roboto', sans-serif; /* Complementary font */
}

.background-bubbles {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 1;
  overflow: hidden;
  pointer-events: none;
}

.particle {
  position: absolute;
  background: radial-gradient(circle, rgba(255,255,255,0.8) 0%, rgba(255,255,255,0) 70%);
  width: 10px;
  height: 10px;
  border-radius: 50%;
  animation: floatUp 5s infinite ease-in-out;
}

@keyframes floatUp {
  0% {
    transform: translateY(0);
    opacity: 1;
  }
  100% {
    transform: translateY(-100vh);
    opacity: 0;
  }
}



.landing-content h1 {
  font-size: 3.5rem;
  font-weight: 800;
  margin-bottom: 1rem;
  background: linear-gradient(to right, #ffffff, #f0f0f0);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  text-shadow: 0 4px 15px rgba(0,0,0,0.2);
  font-family: 'Poppins', sans-serif; /* Main font for headings */
}

.landing-content p {
  font-size: 1.25rem;
  font-weight: 300;
  margin-bottom: 2rem;
  color: rgba(255,255,255,0.8);
  line-height: 1.6;
  font-family: 'Roboto', sans-serif; /* Main font for body text */
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
  font-family: 'Montserrat', sans-serif; /* Font for buttons */
}
