.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 20px 40px;
  background-color: #ffffff;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.logo {
  font-size: 1.8rem;
  font-weight: bold;
  color: #4a90e2;
}

.navbar {
  display: flex;
  gap: 20px;
}

.navbar a {
  text-decoration: none;
  color: #333;
  font-size: 1rem;
  transition: color 0.3s ease;
}

.navbar a:hover {
  color: #4a90e2;
}

.authButtons {
  display: flex;
  gap: 15px;
}

.loginButton,
.signupButton {
  padding: 10px 20px;
  font-size: 0.9rem;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.loginButton {
  background-color: #ffffff;
  color: #4a90e2;
  border: 1px solid #4a90e2;
}

.loginButton:hover {
  background-color: #4a90e2;
  color: #ffffff;
}

.signupButton {
  background-color: #4a90e2;
  color: #ffffff;
}

.signupButton:hover {
  background-color: #357abd;
}




.hero {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 60px 40px;
  background: linear-gradient(135deg, #f2f2f2 0%, #7ca2e1 100%);
  min-height: 80vh;
}

.heroContent {
  max-width: 50%;
}

.heroTitle {
  font-size: 2.8rem;
  font-weight: bold;
  color: #ffffff;
  margin-bottom: 20px;
}

.heroSubtitle {
  font-size: 1.2rem;
  color: #e1e5f2;
  margin-bottom: 30px;
  line-height: 1.6;
}

.ctaButton {
  padding: 15px 30px;
  font-size: 1rem;
  color: #ffffff;
  background-color: #4a90e2;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.ctaButton:hover {
  background-color: #357abd;
}

.heroImage {
  width: 45%;
  height: auto;
  background-image: url('your-hero-image-url.jpg');
  background-size: cover;
  background-position: center;
  border-radius: 12px;
  box-shadow: 0 6px 15px rgba(0, 0, 0, 0.2);
}




.features {
  padding: 60px 40px;
  background-color: #ffffff;
}

.sectionTitle {
  font-size: 2rem;
  font-weight: bold;
  color: #4a90e2;
  text-align: center;
  margin-bottom: 40px;
}

.featureCards {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 20px;
}



.card {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 20px;
  background-color: #f9f9f9;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  text-align: center;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.card:hover {
  transform: translateY(-10px);
  box-shadow: 0 8px 15px rgba(0, 0, 0, 0.2);
}

.icon {
  font-size: 2.5rem;
  color: #4a90e2;
  margin-bottom: 20px;
}

h3 {
  font-size: 1.2rem;
  font-weight: bold;
  color: #333;
  margin-bottom: 10px;
}

p {
  font-size: 1rem;
  color: #666;
  line-height: 1.5;
}



.container {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
  background-color: #f9f9f9;
}
