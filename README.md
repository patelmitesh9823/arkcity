<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>ArkCity C-Store Gas Report (2025)</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background-color: #e6f0ff;
      color: #003366;
      margin: 0;
      padding: 0;
    }
    header {
      background-color: #0059b3;
      color: white;
      padding: 20px;
      text-align: center;
    }
    main {
      padding: 20px;
    }
    .dashboard {
      display: flex;
      flex-wrap: wrap;
      gap: 20px;
      justify-content: space-around;
      margin-bottom: 20px;
    }
    .dashboard div {
      background-color: #cce0ff;
      padding: 20px;
      border-radius: 10px;
      flex: 1 1 250px;
      text-align: center;
    }
    table {
      width: 100%;
      border-collapse: collapse;
      margin-bottom: 20px;
    }
    th, td {
      border: 1px solid #003366;
      padding: 6px;
      text-align: center;
      font-size: 14px;
    }
    th {
      background-color: #99c2ff;
    }
    canvas {
      background-color: #ffffff;
      border: 1px solid #003366;
      border-radius: 10px;
      display: block;
      margin: 20px auto;
    }
    button {
      background-color: #0059b3;
      color: white;
      padding: 8px 16px;
      border: none;
      border-radius: 5px;
      cursor: pointer;
      margin: 5px;
    }
    button:hover {
      background-color: #004080;
    }
    .inputs-box {
      margin: 15px 0;
      text-align: center;
    }
    .inputs-box input {
      width: 100px;
      margin: 5px;
      padding: 5px;
    }
    .month-section {
      border: 2px solid #0059b3;
      border-radius: 10px;
      margin-bottom: 30px;
      padding: 10px;
      background: #f2f6ff;
    }
    .month-section h2 {
      text-align: center;
      background: #0059b3;
      color: white;
      padding: 5px;
      border-radius: 8px;
    }
  </style>
</head>
<body>
  <header>
    <h1>ArkCity C-Store Gas Report (2025)</h1>
  </header>
  <main>
    <!-- Starting Inventory Inputs -->
    <div class="inputs-box">
      <h2>Set Starting Inventory</h2>
      Regular: <input type="number" id="startRegular" value="4000"> 
      Ethanol: <input type="number" id="startEthanol" value="8000"> 
      Premium: <input type="number" id="startPremium" value="4000"> 
      <button onclick="setInventory()">Set Inventory</button>
    </div>

    <!-- Add Delivery Inputs -->
    <div class="inputs-box">
      <h2>Add Inventory (Delivery)</h2>
      Regular: <input type="number" id="addRegular" value="0"> 
      Ethanol: <input type="number" id="addEthanol" value="0"> 
      Premium: <input type="number" id="addPremium" value="0"> 
      <button onclick="addInventory()">Add Delivery</button>
    </div>

    <section class="dashboard">
      <div>
        <h2>Total Sales (Year)</h2>
        <p id="totalSales">0 gal</p>
      </div>
      <div>
        <h2>Remaining Inventory</h2>
        <p id="remainingInventory">Regular: 0, Ethanol: 0, Premium: 0</p>
      </div>
      <div>
        <h2>Summary</h2>
        <p id="dailySummary">-</p>
      </div>
    </section>

    <!-- 12 Month Tables -->
    <div id="monthsContainer"></div>

    <button onclick="updateInventory()">Update Inventory</button>

    <h2>Sales Trend Chart (Full Year)</h2>
    <canvas id="salesChart" width="900" height="400"></canvas>
  </main>

  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <script>
    let inventory = { regular: 0, ethanol: 0, premium: 0 };
    let salesData = [];
    let chart;

    const months = [
      "January","February","March","April","May","June",
      "July","August","September","October","November","December"
    ];

    // Build 12 month tables
    const container = document.get
