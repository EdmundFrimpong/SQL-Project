<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <title>BigQuery Data Exploration - Homelessness Project</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            max-width: 900px;
            margin: 40px auto;
            padding: 0 20px;
            line-height: 1.6;
            color: #333;
            background-color: #fafafa;
        }
        h1, h2, h3 {
            color: #2c3e50;
        }
        code {
            background-color: #eee;
            padding: 2px 6px;
            font-family: monospace;
            border-radius: 4px;
        }
        pre {
            background: #f4f4f4;
            padding: 15px;
            overflow-x: auto;
            border-radius: 6px;
            margin-bottom: 20px;
        }
        table {
            border-collapse: collapse;
            width: 100%;
            margin-bottom: 20px;
        }
        th, td {
            border: 1px solid #ddd;
            padding: 8px;
        }
        th {
            background-color: #2c3e50;
            color: white;
            text-align: left;
        }
        ul {
            margin-top: 0;
        }
        hr {
            margin: 40px 0;
            border: 0;
            border-top: 1px solid #ccc;
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
        <li><strong>Is there any way to determine which state each row of data is located in?</strong><br />
            Yes, you can determine each state by using the state columns which contain the 2-letter abbreviation.
        </li>
        <li><strong>How many total rows of data are there?</strong><br />
            2768
        </li>
        <li><strong>What might be some reasons someone would use this dataset?</strong><br />
            You can use this dataset to gain insight into homelessness patterns, identify areas in need of assistance, track changes over time, and help design support programs.
        </li>
    </ul>

    <h3>Step 3 - Creating a New Table with SQL</h3>
    <p>Run this SQL to create a new table:</p>
    <pre><code>CREATE TABLE Exploration_Project.homelessness AS
SELECT CoC_Number, LEFT(CoC_Number, 2) AS State, CoC_Name, Overall_Homeless, Sheltered_ES_Homeless, Sheltered_TH_Homeless, Sheltered_SH_Homeless, Sheltered_Total_Homeless, Unsheltered_Homeless, Homeless_Individuals, Homeless_People_in_Families, Chronically_Homeless, Homeless_Veterans, Homeless_Unaccompanied_Youth_Under_18, Count_Year
FROM `bigquery-public-data.sdoh_hud_pit_homelessness.hud_pit_by_coc`</code></pre>

    <p><strong>What does <code>LEFT(CoC_Number, 2) AS State</code> do?</strong><br />
        It creates a new column called <code>State</code> which contains the first 2 characters from <code>CoC_Number</code>, representing the state abbreviation.
    </p>

    <h3>Step 4</h3>
    <p>Open the new table and preview the data to confirm the columns are correct.</p>

    <hr />

    <h2>Part 2</h2>
    <h3>Step 1</h3>
    <p>Open the <code>homelessness</code> table and create a new query.</p>

    <h3>Step 2 - Questions & Answers</h3>

    <h4>1. Top 3 locations with highest number of unaccompanied homeless youth under 18 in 2018</h4>
    <p><strong>Answer:</strong></p>
    <ol>
        <li>San Jose/Santa Clara City & County CoC - 506</li>
        <li>Oregon Balance of State CoC - 243</li>
        <li>Las Vegas/Clark County CoC - 214</li>
    </ol>
    <p><strong>SQL used:</strong></p>
    <pre><code>SELECT CoC_Name, Homeless_Unaccompanied_Youth_Under_18
FROM `arboreal-totem-456307-n3.Exploration_Project.homelessness`
WHERE Count_Year = 2018
ORDER BY Homeless_Unaccompanied_Youth_Under_18 DESC
LIMIT 3;</code></pre>

    <h4>2. Has the number of unsheltered homeless people in Delaware ("DE") increased over 7 years?</h4>
    <p><strong>Answer:</strong> Yes, with a 322.73% increase from 2012 to 2018.</p>
    <table>
        <thead>
            <tr><th>Unsheltered_Homeless</th><th>Count_Year</th></tr>
        </thead>
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
    <p><strong>SQL used:</strong></p>
    <pre><code>SELECT Unsheltered_Homeless, Count_Year
FROM `arboreal-totem-456307-n3.Exploration_Project.homelessness`
WHERE State = "DE"
ORDER BY Count_Year DESC
LIMIT 7;</code></pre>

    <h4>3. Safe Haven Program analysis in 2018</h4>
    <p><strong>How many locations had at least 1 person as Sheltered_SH?</strong></p>
    <p>Answer: 89 locations.</p>
    <p><strong>SQL used:</strong></p>
    <pre><code>SELECT CoC_Name, Sheltered_SH_Homeless, Count_Year
FROM `arboreal-totem-456307-n3.Exploration_Project.homelessness`
WHERE Count_Year = 2018 AND Sheltered_SH_Homeless > 1
ORDER BY Sheltered_SH_Homeless;</code></pre>

    <p><strong>Top 3 locations with highest Sheltered_SH in 2018:</strong></p>
    <ol>
        <li>Philadelphia CoC - 235</li>
        <li>Reno, Sparks/Washoe County CoC - 185</li>
        <li>Indianapolis CoC - 68</li>
    </ol>
    <p><strong>SQL used:</strong></p>
    <pre><code>SELECT CoC_Name, Sheltered_SH_Homeless, Count_Year
FROM `arboreal-totem-456307-n3.Exploration_Project.homelessness`
WHERE Count_Year = 2018 AND Sheltered_SH_Homeless > 1
ORDER BY Sheltered_SH_Homeless DESC
LIMIT 3;</code></pre>

    <h4>4. Top 7 states by overall homeless population in 2018</h4>
    <table>
        <thead>
            <tr><th>State</th><th>Total_Homeless</th></tr>
        </thead>
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
    <p><strong>SQL used:</strong></p>
    <pre><code>SELECT State, SUM(Overall_Homeless) AS Total_Homeless
FROM `arboreal-totem-456307-n3.Exploration_Project.homelessness`
WHERE Count_Year = 2018
GROUP BY State
ORDER BY Total_Homeless DESC
LIMIT 7;</code></pre>

    <h4>5. Are the top 7 states by homeless population and total population the same?</h4>
    <p>No. The states that differ are New York, Texas, Washington, Massachusetts, and Oregon.</p>
    <p><strong>Overrepresented states (higher homeless ranking than population ranking):</strong> Washington, Oregon, Massachusetts.</p>

    <h4>6. Locations with more than 1000 overall homeless but less than 100 unsheltered homeless in 2018</h4>
    <table>
        <thead>
            <tr>
                <th>CoC_Name</th>
                <th>State</th>
                <th>Overall_Homeless</th>
                <th>Unsheltered_Homeless</th>
            </tr>
        </thead>
        <tbody>
            <tr><td>Anchorage CoC</td><td>AK</td><td>1094</td><td>94</td></tr>
            <tr><td>Delaware Statewide CoC</td><td>DE</td><td>1082</td><td>93</td></tr>
            <tr><td>Lynn CoC</td><td>MA</td><td>1061</td><td>42</td></tr>
            <tr><td>Springfield/Hampden County CoC</td><td>MA</td><td>1109</td><td>93</td></tr>
            <tr><td>East Boston CoC</td><td>MA</td><td>1235</td><td>92</td></tr>
        </tbody>
    </table>
    <p><strong>SQL used:</strong></p>
    <pre><code>SELECT CoC_Name, State, Overall_Homeless, Unsheltered_Homeless
FROM `arboreal-totem-456307-n3.Exploration_Project.homelessness`
WHERE Overall_Homeless > 1000 AND Unsheltered_Homeless < 100 AND Count_Year = 2018
ORDER BY Overall_Homeless DESC;</code></pre>

    <h4>7. How many records in total?</h4>
    <p>2768</p>

    <h4>8. Number of unique states</h4>
    <p>56</p>

    <hr />

    <h2>Summary of Learnings</h2>
    <ul>
        <li>BigQuery SQL can be used to clean, aggregate, and analyze homelessness data efficiently.</li>
        <li>Using simple string functions like <code>LEFT()</code>, you can derive new columns such as state codes.</li>
        <li>Aggregations and filtering help uncover key insights about population trends and demographics.</li>
        <li>Data exploration is key to understanding datasets before analysis.</li>
    </ul>

</body>
</html>
