import React, { useState } from "react";

const Login = () => {
  const [isLogin, setIsLogin] = useState(true);

  const toggleForm = () => {
    setIsLogin(!isLogin);
  };

  return (
    <div className="flex h-screen bg-gradient-to-r from-indigo-500 to-purple-500">
      {/* Left side - Image */}
      <div className="hidden md:flex w-1/2 items-center justify-center bg-cover bg-center" 
           style={{ backgroundImage: `url('https://source.unsplash.com/random/800x800?tech')` }}>
        {/* You can replace the URL with your own image */}
        <div className="text-white text-3xl font-bold p-6 text-center bg-black bg-opacity-50 rounded-md">
          Welcome Back!
        </div>
      </div>

      {/* Right side - Login form */}
      <div className="flex flex-col justify-center w-full md:w-1/2 p-8 bg-white rounded-lg shadow-lg">
        <h2 className="text-3xl font-bold mb-6 text-center text-indigo-600">
          {isLogin ? "Login" : "Register"}
        </h2>

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
      </div>
    </div>
  );
};

export default Login;
