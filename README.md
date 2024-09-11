/* Default Light Theme */
:root {
  --footer-background: linear-gradient(to bottom, #f0f0f5, #dcdcdc);
  --footer-border-color: rgba(15, 95, 220, 1);
  --footer-text-color: #333;
  --footer-heading-color: #333;
  --card-background: linear-gradient(90deg, #6f36cd 0%, #1f77f6 100%);
  --card-text-color: #fcfcfc;
  --card-hover-text-color: #ff80bf;
  --card-link-color: #fff;
  --footer-info-color: #888;
  --footer-info-hover-color: #ff80bf;
}

/* Dark Theme */
[data-theme="dark"] {
  --footer-background: linear-gradient(to bottom, #1a1a2e, #16213e);
  --footer-border-color: rgba(219, 197, 255, 1);
  --footer-text-color: #fcfcfc;
  --footer-heading-color: #fff;
  --card-background: linear-gradient(90deg, #9d66f5 0%, #c1a1f2 100%);
  --card-text-color: #fff;
  --card-hover-text-color: #ff80bf;
  --card-link-color: #fff;
  --footer-info-color: #ccc;
  --footer-info-hover-color: #ff80bf;
}

/* Footer Styles */
.footer {
  background: var(--footer-background);
  border-top: 0.1px solid var(--footer-border-color);
  padding: 40px 20px;
  text-align: center;
  color: var(--footer-text-color);
  margin: 30px -88px;
  margin-top: 100px;
  margin-bottom: 0px;
  position: relative;
  overflow: hidden;
}

.footerHeading {
  font-size: 28px;
  margin-bottom: 40px;
  color: var(--footer-heading-color);
  margin-top: 0px;
  letter-spacing: 2px;
  padding-bottom: 10px;
}

/* Card Styles */
.card {
  background: var(--card-background);
  border-radius: 12px;
  padding: 20px 15px;
  width: 240px;
  color: var(--card-text-color);
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.2);
  transition: transform 0.3s ease, box-shadow 0.3s ease;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  text-align: left;
  position: relative;
}

.card:hover {
  transform: translateY(-10px);
  box-shadow: 0 20px 30px rgba(0, 0, 0, 0.3);
}

.cardTitle {
  font-size: 18px;
  font-weight: bold;
  margin-bottom: 10px;
  color: var(--card-text-color);
}

.cardDescription {
  font-size: 13px;
  font-weight: 500;
  color: var(--card-text-color);
  margin-top: 0px;
  margin-bottom: 20px;
}

.cardLink {
  margin-top: auto;
  color: var(--card-link-color);
  text-decoration: none;
  font-weight: bold;
  display: inline-flex;
  align-items: center;
  gap: 5px;
  font-size: 16px;
  transition: color 0.3s ease;
}

.cardLink:hover {
  color: var(--card-hover-text-color);
}

.footerInfo {
  margin-top: 30px;
  font-size: 14px;
  color: var(--footer-info-color);
}

.footerInfo p:hover {
  color: var(--footer-info-hover-color);
}
