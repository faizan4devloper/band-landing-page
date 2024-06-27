import React from "react";
import { Line } from "react-chartjs-2";

const AdmissionChart = () => {
  const data = {
    labels: ["2020-2021", "2021-2022", "2022-2023", "2023-2024", "2024-2025"],
    datasets: [
      {
        label: "# of Applications Received",
        data: [800, 1000, 1200, 1500, 2000],
        borderColor: "rgba(75, 192, 192, 1)",
        backgroundColor: "rgba(75, 192, 192, 0.2)",
        borderWidth: 1,
      },
      {
        label: "# of Applications Accepted",
        data: [250, 300, 300, 350, 380],
        borderColor: "rgba(255, 99, 132, 1)",
        backgroundColor: "rgba(255, 99, 132, 0.2)",
        borderWidth: 1,
      },
    ],
  };

  const options = {
    scales: {
      yAxes: [
        {
          ticks: {
            beginAtZero: true,
          },
        },
      ],
    },
  };

  return (
    <div className="chart">
      <h5>Admission Trend Chart</h5>
      <Line data={data} options={options} />
    </div>
  );
};

export default AdmissionChart;
import React from "react";
import { Line } from "react-chartjs-2";

const AppealChart = () => {
  const data = {
    labels: ["2020-2021", "2021-2022", "2022-2023", "2023-2024", "2024-2025"],
    datasets: [
      {
        label: "# of Appeals Received",
        data: [20, 30, 35, 40, 43],
        borderColor: "rgba(75, 192, 192, 1)",
        backgroundColor: "rgba(75, 192, 192, 0.2)",
        borderWidth: 1,
      },
      {
        label: "# of Appeals Accepted",
        data: [4, 6, 8, 10, 12],
        borderColor: "rgba(255, 99, 132, 1)",
        backgroundColor: "rgba(255, 99, 132, 0.2)",
        borderWidth: 1,
      },
    ],
  };

  const options = {
    scales: {
      yAxes: [
        {
          ticks: {
            beginAtZero: true,
          },
        },
      ],
    },
  };

  return (
    <div className="chart">
      <h5>Appeal Trend Chart</h5>
      <Line data={data} options={options} />
    </div>
  );
};

export default AppealChart;
