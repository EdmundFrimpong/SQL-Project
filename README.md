<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>BigQuery Homelessness Project</title>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap" rel="stylesheet">
  <style>
    * {
      box-sizing: border-box;
      scroll-behavior: smooth;
    }

    body {
      font-family: 'Inter', sans-serif;
      margin: 0 auto;
      padding: 20px;
      max-width: 1000px;
      line-height: 1.8;
      color: #2c3e50;
      background: #f9fafb;
      animation: fadeIn 1.2s ease-in;
    }

    h1, h2, h3, h4 {
      color: #1a202c;
    }

    h1 {
      font-size: 2.2rem;
      margin-top: 30px;
    }

    h2 {
      font-size: 1.6rem;
      margin-top: 40px;
    }

    h3, h4 {
      font-size: 1.2rem;
      margin-top: 25px;
    }

    p, li {
      font-size: 1rem;
    }

    code {
      background-color: #e2e8f0;
      padding: 3px 6px;
      border-radius: 4px;
      font-family: monospace;
    }

    pre {
      background: #f1f5f9;
      border-left: 4px solid #4299e1;
      padding: 15px;
      overflow-x: auto;
      border-radius: 6px;
      margin-bottom: 20px;
    }

    table {
      width: 100%;
      border-collapse: collapse;
      margin: 20px 0;
      font-size: 0.95rem;
    }

    th, td {
      border: 1px solid #cbd5e0;
      padding: 10px;
      text-align: left;
    }

    th {
      background-color: #2b6cb0;
      color: white;
    }

    tr:nth-child(even) {
      background-color: #edf2f7;
    }

    ul {
      padding-left: 20px;
    }

    ol {
      padding-left: 25px;
    }

    hr {
      border: none;
      border-top: 1px solid #cbd5e0;
      margin: 50px 0;
    }

    @keyframes fadeIn {
      from { opacity: 0; transform: translateY(20px); }
      to { opacity: 1; transform: translateY(0); }
    }

    @media (max-width: 768px) {
      body {
        padding: 15px;
      }

      h1 {
        font-size: 1.6rem;
      }

      h2 {
        font-size: 1.3rem;
      }
    }
  </style>
</head>
<body>

  <h1>BigQuery Data Exploration - Homelessness Project</h1>

  <h2>Part 1</h2>
  <h3>Step 1</h3>
  <p>While exploring the dataset, look at the Schema tab and read through the descriptions provided for each column, to get a sense of what data is in this table. This will help you answer the questions below.</p>

  <h3>Step 2 - Questions & Answers</h3>
  <ul>
    <li><strong>What do these acronyms stand for?</strong><br />
      CoC = Continuum of Care<br />
      Sheltered_ES = Homeless in Emergency Shelters<br />
      Sheltered_TH = Homeless in Transitional Housing<br />
      Sheltered_SH = Homeless in Safe Haven Housing
    </li>
    <li><strong>What are the only 3 columns that are NOT an Integer type?</strong><br />
      CoC_Number, CoC_Name, CoC_Category
    </li>
    <li><strong>Can we determine the state of each row?</strong><br />
      Yes, by extracting from CoC_Number or using the state column.
    </li>
    <li><strong>Total rows:</strong> 2768</li>
    <li><strong>Use cases:</strong> Understand homelessness patterns, plan aid, policy-making.</li>
  </ul>

  <h3>Step 3 - Create Table SQL</h3>
  <pre><code>CREATE TABLE Exploration_Project.homelessness AS
SELECT CoC_Number, LEFT(CoC_Number, 2) AS State, CoC_Name, Overall_Homeless, 
  Sheltered_ES_Homeless, Sheltered_TH_Homeless, Sheltered_SH_Homeless, 
  Sheltered_Total_Homeless, Unsheltered_Homeless, Homeless_Individuals, 
  Homeless_People_in_Families, Chronically_Homeless, Homeless_Veterans, 
  Homeless_Unaccompanied_Youth_Under_18, Count_Year
FROM `bigquery-public-data.sdoh_hud_pit_homelessness.hud_pit_by_coc`</code></pre>

  <p><strong>LEFT(CoC_Number, 2):</strong> Extracts first 2 characters (state abbreviation).</p>

  <h3>Step 4</h3>
  <p>Preview the new table and confirm column creation.</p>

  <hr />

  <h2>Part 2</h2>
  <h3>Top 3 locations for unaccompanied homeless youth in 2018</h3>
  <ol>
    <li>San Jose/Santa Clara City & County CoC - 506</li>
    <li>Oregon Balance of State CoC - 243</li>
    <li>Las Vegas/Clark County CoC - 214</li>
  </ol>

  <pre><code>SELECT CoC_Name, Homeless_Unaccompanied_Youth_Under_18
FROM `...`
WHERE Count_Year = 2018
ORDER BY Homeless_Unaccompanied_Youth_Under_18 DESC
LIMIT 3;</code></pre>

  <h3>Unsheltered Homeless in Delaware (2012–2018)</h3>
  <table>
    <thead><tr><th>Unsheltered</th><th>Year</th></tr></thead>
    <tbody>
      <tr><td>93</td><td>2018</td></tr>
      <tr><td>58</td><td>2017</td></tr>
      <tr><td>51</td><td>2016</td></tr>
      <tr><td>37</td><td>2015</td></tr>
      <tr><td>37</td><td>2014</td></tr>
      <tr><td>10</td><td>2013</td></tr>
      <tr><td>22</td><td>2012</td></tr>
    </tbody>
  </table>

  <h3>Safe Haven Program - 2018</h3>
  <p><strong>Locations with >1 Sheltered_SH:</strong> 89</p>
  <ol>
    <li>Philadelphia CoC - 235</li>
    <li>Reno/Sparks/Washoe CoC - 185</li>
    <li>Indianapolis CoC - 68</li>
  </ol>

  <h3>Top 7 States by Homeless Population - 2018</h3>
  <table>
    <thead><tr><th>State</th><th>Total Homeless</th></tr></thead>
    <tbody>
      <tr><td>CA</td><td>129,972</td></tr>
      <tr><td>NY</td><td>91,897</td></tr>
      <tr><td>FL</td><td>31,030</td></tr>
      <tr><td>TX</td><td>25,310</td></tr>
      <tr><td>WA</td><td>22,304</td></tr>
      <tr><td>MA</td><td>20,068</td></tr>
      <tr><td>OR</td><td>14,476</td></tr>
    </tbody>
  </table>

  <h3>Unique states in dataset:</h3>
  <p><strong>56</strong></p>

  <hr />

  <h2>Summary</h2>
  <ul>
    <li>SQL enables data cleaning, transformation, and analysis.</li>
    <li>Aggregations reveal trends in homelessness across states.</li>
    <li>Simple functions like <code>LEFT()</code> extract useful info.</li>
    <li>BigQuery is efficient for large public datasets.</li>
  </ul>
</body>
</html>
