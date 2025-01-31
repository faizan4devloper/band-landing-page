import React, { useState, useEffect } from "react";
import axios from "axios";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { 
  faChartLine, 
  faClipboardList, 
  faChartPie, 
  faCalendarCheck 
} from "@fortawesome/free-solid-svg-icons";
import styles from "./Summary.module.css";

const API_ENDPOINT = "https://your-api-url.com"; // Replace with actual API

const Summary = () => {
  const [tile1Data, setTile1Data] = useState([]);
  const [tile2Data, setTile2Data] = useState({ total: 0, successful: 0 });
  const [tile3Data, setTile3Data] = useState({ total: 0, successful: 0 });
  const [tile4Data, setTile4Data] = useState({ total: 0, successful: 0 });

  // Generic function for fetching API data
  const fetchData = async (payload, setData) => {
    try {
      const response = await axios.post(API_ENDPOINT, payload, {
        headers: { "Content-Type": "application/json" },
      });

      if (response.data) {
        setData(response.data);
      }
    } catch (error) {
      console.error("Error fetching data:", error);
    }
  };

  // Fetch pagination data (First 3 pages with refresh, then without)
  useEffect(() => {
    const fetchPaginationData = async () => {
      try {
        let allData = [];
        for (let i = 1; i <= 3; i++) {
          const payload = {
            action_type: "pagination",
            refresh: i === 1, // Refresh only on the first page
            start_page: i,
            page_size: 20,
          };
          const response = await axios.post(API_ENDPOINT, payload);
          if (response.data) {
            allData = [...allData, ...response.data];
          }
        }
        setTile1Data(allData);
      } catch (error) {
        console.error("Error fetching pagination data:", error);
      }
    };

    fetchPaginationData();
  }, []);

  // Fetch tile data
  useEffect(() => {
    fetchData({ action_type: "transaction_counts", time_period: "previous_day" }, setTile2Data);
    fetchData({ action_type: "transaction_counts", time_period: "day_before_yesterday" }, setTile3Data);
    fetchData({ action_type: "transaction_counts", time_period: "last_month" }, setTile4Data);
  }, []);

  return (
    <div className={styles.summaryContainer}>
      {/* Tile 1 - Pagination Data */}
      <div className={`${styles.tile} ${styles.tile1}`}>
        <div className={styles.tileHeader}>
          <FontAwesomeIcon icon={faChartLine} className={styles.tileIcon} />
          <h4>Pagination Data</h4>
        </div>
        <div className={styles.tileContent}>
          <ul>
            {tile1Data.map((item, index) => (
              <li key={index}>{item.date} - {item.time}</li>
            ))}
          </ul>
        </div>
      </div>

      {/* Tile 2 - Previous Day Transactions */}
      <div className={`${styles.tile} ${styles.tile2}`}>
        <div className={styles.tileHeader}>
          <FontAwesomeIcon icon={faClipboardList} className={styles.tileIcon} />
          <h4>Previous Day Transactions</h4>
        </div>
        <div className={styles.tileContent}>
          <div className={styles.statItem}>
            <span>Total</span>
            <span className={styles.statValue}>{tile2Data.total}</span>
          </div>
          <div className={styles.statItem}>
            <span>Successful</span>
            <span className={styles.statValue}>{tile2Data.successful}</span>
          </div>
        </div>
      </div>

      {/* Tile 3 - Day Before Yesterday Transactions */}
      <div className={`${styles.tile} ${styles.tile3}`}>
        <div className={styles.tileHeader}>
          <FontAwesomeIcon icon={faChartPie} className={styles.tileIcon} />
          <h4>Day Before Yesterday Transactions</h4>
        </div>
        <div className={styles.tileContent}>
          <div className={styles.statItem}>
            <span>Total</span>
            <span className={styles.statValue}>{tile3Data.total}</span>
          </div>
          <div className={styles.statItem}>
            <span>Successful</span>
            <span className={styles.statValue}>{tile3Data.successful}</span>
          </div>
        </div>
      </div>

      {/* Tile 4 - Last Month Transactions */}
      <div className={`${styles.tile} ${styles.tile4}`}>
        <div className={styles.tileHeader}>
          <FontAwesomeIcon icon={faCalendarCheck} className={styles.tileIcon} />
          <h4>Last Month Transactions</h4>
        </div>
        <div className={styles.tileContent}>
          <div className={styles.statItem}>
            <span>Total</span>
            <span className={styles.statValue}>{tile4Data.total}</span>
          </div>
          <div className={styles.statItem}>
            <span>Successful</span>
            <span className={styles.statValue}>{tile4Data.successful}</span>
          </div>
        </div>
      </div>
    </div>
  );
};

export default Summary;
