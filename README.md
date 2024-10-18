import React from "react";
import LoginPage from "./LoginPage";

function App() {
  return (
    <div className="App">
      <LoginPage />
    </div>
  );
}

export default App;



import React, { useState } from "react";
import { motion } from "framer-motion";

const LoginPage = () => {
  const [isLogin, setIsLogin] = useState(true);

  const containerVariants = {
    hidden: { opacity: 0, x: "-100vw" },
    visible: { opacity: 1, x: 0, transition: { duration: 0.5, type: "spring", stiffness: 120 } },
    exit: { opacity: 0, x: "100vw", transition: { ease: "easeInOut" } },
  };

  const toggleForm = () => {
    setIsLogin(!isLogin);
  };

  return (
    <div className="flex justify-center items-center h-screen bg-gradient-to-r from-indigo-500 to-purple-500">
      <motion.div
        className="bg-white p-8 rounded-lg shadow-lg w-80"
        variants={containerVariants}
        initial="hidden"
        animate="visible"
        exit="exit"
      >
        <h2 className="text-3xl font-bold mb-6 text-center text-indigo-600">{isLogin ? "Login" : "Register"}</h2>

        <form>
          <div className="mb-4">
            <label className="block text-gray-700 text-sm font-bold mb-2" htmlFor="email">
              Email
            </label>
            <input
              id="email"
              type="email"
              className="w-full px-3 py-2 border border-gray-300 rounded-lg focus:outline-none focus:border-indigo-500"
              placeholder="Enter your email"
            />
          </div>

          <div className="mb-6">
            <label className="block text-gray-700 text-sm font-bold mb-2" htmlFor="password">
              Password
            </label>
            <input
              id="password"
              type="password"
              className="w-full px-3 py-2 border border-gray-300 rounded-lg focus:outline-none focus:border-indigo-500"
              placeholder="Enter your password"
            />
          </div>

          <div className="flex items-center justify-between">
            <button
              type="button"
              className="bg-indigo-500 text-white font-bold py-2 px-4 rounded focus:outline-none hover:bg-indigo-600 w-full"
            >
              {isLogin ? "Login" : "Sign Up"}
            </button>
          </div>
        </form>

        <div className="mt-6 text-center">
          <button
            type="button"
            className="text-indigo-500 hover:text-indigo-700 font-semibold"
            onClick={toggleForm}
          >
            {isLogin ? "Create an Account" : "Already have an account? Login"}
          </button>
        </div>
      </motion.div>
    </div>
  );
};

export default LoginPage;
