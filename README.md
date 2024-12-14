README: SQL Code for Analyzing Manchester United's 2014/2015 Losses
This SQL query analyzes the performance of Manchester United (MU) during the 2014/2015 season, specifically focusing on the matches they lost, whether at home or away. It uses Common Table Expressions (CTEs) to identify outcomes for both the home and away teams and ranks matches based on goal difference.

Objectives
Identify matches where Manchester United lost, either as the home or away team.
Retrieve match details, including date, opposing teams, goals scored, and the ranking of matches by goal difference (largest differences ranked highest).
Code Overview
1. CTE: home
This CTE identifies outcomes when Manchester United played as the home team.

Columns:
id: Match ID.
team_long_name: The name of the home team.
outcome: Outcome of the match for the home team:
MU Win: Home goals > Away goals.
MU Loss: Home goals < Away goals.
Tie: Home goals = Away goals.
SQL Snippet:

sql
Copy code
WITH home AS (  
  SELECT m.id, t.team_long_name,  
    CASE WHEN m.home_goal > m.away_goal THEN 'MU Win'  
         WHEN m.home_goal < m.away_goal THEN 'MU Loss'  
         ELSE 'Tie' END AS outcome  
  FROM match AS m  
  LEFT JOIN team AS t ON m.hometeam_id = t.team_api_id)  
2. CTE: away
This CTE identifies outcomes when Manchester United played as the away team.

Columns:
id: Match ID.
team_long_name: The name of the away team.
outcome: Outcome of the match for the away team:
MU Win: Away goals > Home goals.
MU Loss: Away goals < Home goals.
Tie: Away goals = Home goals.
SQL Snippet:

sql
Copy code
away AS (  
  SELECT m.id, t.team_long_name,  
    CASE WHEN m.home_goal > m.away_goal THEN 'MU Loss'  
         WHEN m.home_goal < m.away_goal THEN 'MU Win'  
         ELSE 'Tie' END AS outcome  
  FROM match AS m  
  LEFT JOIN team AS t ON m.awayteam_id = t.team_api_id)  
3. Main Query
The main query combines the home and away CTEs to retrieve details of Manchester United’s losses, whether at home or away, during the 2014/2015 season.

Key Features:

Columns Selected:

date: Date of the match.
home_team: The home team’s name.
away_team: The away team’s name.
home_goal, away_goal: Goals scored by each team.
match_rank: Rank of the match based on absolute goal difference, with larger differences ranked higher.
Joins:

Matches the home and away CTEs with the match table on id.
Filters:

Only matches from the 2014/2015 season.
Matches where Manchester United lost, either as the home or away team.
SQL Snippet:

sql
Copy code
SELECT DISTINCT  
    m.date,  
    home.team_long_name AS home_team,  
    away.team_long_name AS away_team,  
    m.home_goal, m.away_goal,  
    RANK() OVER (ORDER BY ABS(home_goal - away_goal) DESC) as match_rank  
FROM match AS m  
LEFT JOIN home ON m.id = home.id  
LEFT JOIN away ON m.id = away.id  
WHERE m.season = '2014/2015'  
  AND ((home.team_long_name = 'Manchester United' AND home.outcome = 'MU Loss')  
  OR (away.team_long_name = 'Manchester United' AND away.outcome = 'MU Loss'));  
Query Logic
CTEs (home and away):

These classify the outcome of each match for both home and away teams.
Allows independent calculations for home and away scenarios.
Match Filtering:

Focuses only on Manchester United’s losses using the WHERE clause.
Includes matches where:
Manchester United was the home team and lost.
Manchester United was the away team and lost.
Ranking by Goal Difference:

Uses RANK() with ORDER BY ABS(home_goal - away_goal) DESC to rank matches based on the absolute difference in goals.
Larger goal differences have higher ranks.
Expected Output
Date	Home_Team	Away_Team	Home_Goal	Away_Goal	Match_Rank
2015-03-01	Manchester United	Chelsea	1	3	1
2015-04-10	Arsenal	Manchester United	2	0	2
Use Cases
Analyze Manchester United’s losses to identify trends or areas of weakness.
Rank matches by their competitiveness or goal differences.
Generate detailed reports for specific seasons or match outcomes.
Notes
Ensure the match and team tables contain accurate and complete data.
This query is specific to the 2014/2015 season and Manchester United. To adapt for other seasons or teams, modify the season and team filter conditions.
