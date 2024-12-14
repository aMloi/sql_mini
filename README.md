# README: SQL Code for Analyzing Manchester United's 2014/2015 Losses  

This SQL query analyzes the performance of **Manchester United (MU)** during the 2014/2015 season, specifically focusing on the matches they lost, whether at home or away. It uses **Common Table Expressions (CTEs)** to identify outcomes for both the home and away teams and ranks matches based on goal difference.  

---

## **Objectives**  
- Identify matches where Manchester United lost, either as the home or away team.  
- Retrieve match details, including:  
  - Date  
  - Opposing teams  
  - Goals scored  
  - Ranking of matches by goal difference (largest differences ranked highest).  

---

## **Code Overview**  

### **1. CTE: `home`**  
This CTE identifies outcomes when Manchester United played as the **home team**.  
- **Columns**:  
  - `id`: Match ID.  
  - `team_long_name`: The name of the home team.  
  - `outcome`: Outcome of the match for the home team:  
    - **`MU Win`**: Home goals > Away goals.  
    - **`MU Loss`**: Home goals < Away goals.  
    - **`Tie`**: Home goals = Away goals.  

**SQL Snippet:**  
```sql  
WITH home AS (  
  SELECT m.id, t.team_long_name,  
    CASE WHEN m.home_goal > m.away_goal THEN 'MU Win'  
         WHEN m.home_goal < m.away_goal THEN 'MU Loss'  
         ELSE 'Tie' END AS outcome  
  FROM match AS m  
  LEFT JOIN team AS t ON m.hometeam_id = t.team_api_id)
2. How It Works
CTE: home

Identifies matches where Manchester United played as the home team.
Determines the match outcome:
MU Win: Home goals > Away goals.
MU Loss: Home goals < Away goals.
Tie: Home goals = Away goals.
CTE: away

Identifies matches where Manchester United played as the away team.
Determines the match outcome:
MU Win: Away goals > Home goals.
MU Loss: Away goals < Home goals.
Tie: Away goals = Home goals.
Final Query

Combines the home and away CTEs with the match table using LEFT JOIN.
Filters matches played during the 2014/2015 season where Manchester United lost, either as the home or away team.
Adds a ranking of matches based on the absolute goal difference using the RANK() function.
3. Output Columns
date: Date of the match.
home_team: Name of the home team.
away_team: Name of the away team.
home_goal: Goals scored by the home team.
away_goal: Goals scored by the away team.
match_rank: Rank of the match based on the absolute goal difference.
4. Example Output
Date	Home Team	Away Team	Home Goal	Away Goal	Match Rank
2014-11-22	Manchester United	Arsenal	1	3	1
2015-02-07	Chelsea	Manchester United	0	2	2
2015-05-09	Manchester United	Everton	1	2	3
Key Techniques Used
CTEs (Common Table Expressions)

Simplify complex queries by creating intermediate result sets (home and away).
Window Function: RANK()

Used to rank matches based on the absolute goal difference.
Filtering by Conditions

Ensures only losses by Manchester United in the 2014/2015 season are included.
Usage
Use this query to:

Analyze team performance for specific seasons.
Identify key matches with the largest goal differences.
Gain insights into Manchester United's losses during the 2014/2015 season
