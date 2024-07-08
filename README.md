
.mainContent {
  display: flex;
  flex-direction: column;
  width: 100%;
  padding: 0px 20px;
  background-color: #ffffff;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  height: calc(100vh - 100px); /* Adjust height as needed */
  overflow-y: auto; /* Enable vertical scroll */
}


.mainContent h2 {
  font-size: 20px;
  font-weight: 600;
  color: #5f1ec1;
  margin-bottom: 10px;
  margin-top: 0;
  border-bottom: 2px solid rgba(95, 30, 193, 0.8);
  padding-bottom: 5px;
}

.mainContent ul {
  list-style-type: disc;
  margin-left: 20px;
  padding-left: 20px;
}

.mainContent ul li {
  font-size: 12px;
  line-height: 1.6;
  color: #000;
  margin-bottom: 10px;
}

.mainContent img {
  max-width: 100%;
  height: auto;
  display: block;
  margin: 20px 0;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.benefits, .description, .demo, .architecture, .adoption, .solution{
  padding: 10px 15px;
  background-color: #f9f9f9;
  border-left: 4px solid rgba(95, 30, 193, 0.8);
  margin-bottom: 20px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  margin-bottom: 50px;
}

.benefits p, .description li {
  margin: 0;
  font-size: 12px;
}

.benefits h2, .description h2 {
  font-size: 20px;
  font-weight: 600;
  color: #5f1ec1;
  margin-bottom: 10px;
}

.adoptionTable {
  width: 100%;
  border-collapse: collapse;
  margin-top: 10px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  background-color: #fff;
}

.adoptionTable th, .adoptionTable td {
  border: 1px solid #ddd;
  font-size: 12px;
  padding: 10px;
  text-align: left;
  vertical-align: top;
}

.adoptionTable th {
  background-color: #5f1ec1;
  color: #fff;
  text-transform: uppercase;
  letter-spacing: 1px;
  font-weight: bold;
}

.adoptionTable tr:nth-child(even) {
  background-color: #f9f9f9;
}

.adoptionTable tr:hover {
  background-color: #f1f1f1;
}

.adoptionTable td:first-child {
  font-weight: bold;
  color: #5f1ec1;
}

.highlight{
  font-style: italic;
  /*font-weight: bold;*/
  color: #DD9313;
  /*color: #7C2917;*/
}
