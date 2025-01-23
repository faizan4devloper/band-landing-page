import React, { useState, useEffect } from 'react';
import axios from 'axios';
import styles from './Summary.module.css';

const Summary = () => {
  const [tile1Data, setTile1Data] = useState([]);
  const [tile2Data, setTile2Data] = useState({ total: 0, successful: 0 });
  const [tile3Data, setTile3Data] = useState({ total: 0, successful: 0 });
  const [tile4Data, setTile4Data] = useState({ total: 0, successful: 0 });
  const [currentPage, setCurrentPage] = useState(1);

  // APIs
  const tile1Api = 'API_FOR_TILE_1'; // Replace with actual API endpoint
  const tile2Api = 'API_FOR_TILE_2';
  const tile3Api = 'API_FOR_TILE_3';
  const tile4Api = 'API_FOR_TILE_4';

  // Fetch data for Tile 1
  useEffect(() => {
    const fetchTile1Data = async () => {
      try {
        const responses = await Promise.all(
          Array.from({ length: 3 }, (_, i) =>
            axios.get(`${tile1Api}?page=${i + 1}`)
          )
        );
        setTile1Data(responses.map((res) => res.data));
      } catch (error) {
        console.error('Error fetching Tile 1 data:', error);
      }
    };
    fetchTile1Data();
  }, [tile1Api]);

  // Fetch data for Tile 2
  useEffect(() => {
    const fetchTile2Data = async () => {
      try {
        const response = await axios.get(tile2Api);
        setTile2Data(response.data);
      } catch (error) {
        console.error('Error fetching Tile 2 data:', error);
      }
    };
    fetchTile2Data();
  }, [tile2Api]);

  // Fetch data for Tile 3
  useEffect(() => {
    const fetchTile3Data = async () => {
      try {
        const response = await axios.get(tile3Api);
        setTile3Data(response.data);
      } catch (error) {
        console.error('Error fetching Tile 3 data:', error);
      }
    };
    fetchTile3Data();
  }, [tile3Api]);

  // Fetch data for Tile 4
  useEffect(() => {
    const fetchTile4Data = async () => {
      try {
        const response = await axios.get(tile4Api);
        setTile4Data(response.data);
      } catch (error) {
        console.error('Error fetching Tile 4 data:', error);
      }
    };
    fetchTile4Data();
  }, [tile4Api]);

  return (
    <div className={styles.summaryContainer}>
      {/* Tile 1 */}
      <div className={styles.tile}>
        <h4>Tile 1: Pagination (Date and Time)</h4>
        <ul>
          {tile1Data.flatMap((page, index) =>
            page.map((item, idx) => (
              <li key={`${index}-${idx}`}>{item.date} - {item.time}</li>
            ))
          )}
        </ul>
      </div>

      {/* Tile 2 */}
      <div className={styles.tile}>
        <h4>Tile 2: Today's Transactions</h4>
        <p>Total: {tile2Data.total}</p>
        <p>Successful: {tile2Data.successful}</p>
      </div>

      {/* Tile 3 */}
      <div className={styles.tile}>
        <h4>Tile 3: Today's Transaction Counts</h4>
        <p>Total: {tile3Data.total}</p>
        <p>Successful: {tile3Data.successful}</p>
      </div>

      {/* Tile 4 */}
      <div className={styles.tile}>
        <h4>Tile 4: Last Month's Transactions</h4>
        <p>Total: {tile4Data.total}</p>
        <p>Successful: {tile4Data.successful}</p>
      </div>
    </div>
  );
};

export default Summary;




.summaryContainer {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  gap: 16px;
  padding: 16px;
  background-color: #f9f9f9;
}

.tile {
  background: #fff;
  padding: 16px;
  border: 1px solid #ddd;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.tile h4 {
  margin-bottom: 8px;
  font-size: 16px;
  font-weight: bold;
}

.tile p {
  margin: 4px 0;
  font-size: 14px;
}

.tile ul {
  padding-left: 16px;
  list-style: disc;
}
