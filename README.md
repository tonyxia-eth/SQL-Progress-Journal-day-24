# SQL-Progress-Journal-day-24

# Project: Moneyball Analysis — ROI Based on Home Runs (2001)

## 🎯 Objective
Identify the top 50 most **efficient MLB players** in 2001 based on **ROI** calculated as:

> ROI = (Home Runs / Salary) * 100,000

The scaling factor `100,000` was used to normalize the data for easier interpretation, since salaries are in the 6-figure range. This effectively tells us:  
**"How many home runs per $100,000 of salary"** each player delivered.

---

## 🔍 SQL Query Used

```sql
WITH top_50_players AS (
    SELECT pf.player_id, HR, pf.year, s.salary, p.first_name, p.last_name, 
           ROUND((1.0 * HR) / s.salary * 100000, 4) AS efficiency_percentage
    FROM performances pf
    JOIN salaries s ON pf.player_id = s.player_id AND pf.year = s.year
    JOIN players p ON s.player_id = p.id
    WHERE pf.year = 2001
    ORDER BY efficiency_percentage DESC
    LIMIT 50
)
SELECT * FROM top_50_players;


🧹 Data Cleaning & Name Deduplication
We discovered Tableau couldn’t sort the efficiency_percentage field properly due to duplicate first names. To resolve this:

Identified duplicates within the top 50 players:


WITH top_50_players AS (
    ...
)
SELECT first_name, player_id, COUNT(*)
FROM top_50_players
GROUP BY first_name
HAVING COUNT(*) > 1;


Renamed selected players by updating their first_name values:

UPDATE players SET first_name = 'Ben_1' WHERE id = 14654;
UPDATE players SET first_name = 'Brian_1' WHERE id = 4305;
UPDATE players SET first_name = 'Carlos_1' WHERE id = 10600;

📤 Export to Tableau
Data was exported from sqlite3 using:

.headers on
.mode csv
.once top_50_efficiency.csv
<query here>


Uploaded to Tableau Public

Created a bar chart ordered by ROI efficiency

Used a calculated field to create Full Name = [first_name] + " " + [last_name]

🖼️ Visualization Sample
📌 (Image exported from Tableau and shared below)
(Include your image here if embedding in GitHub repo or README.md)

🛠️ Tools Used
SQLite3 (for SQL queries and exporting)

Tableau Public (for visualization)

VS Code (for editing and troubleshooting)

GitHub.dev (online code environment)

✅ Next Step
In Part B, we’ll:

Compare ROI across multiple performance metrics (e.g., hits, RBIs)

Possibly incorporate data across 1999–2001

Add dashboard features like filters and slicers in Tableau

