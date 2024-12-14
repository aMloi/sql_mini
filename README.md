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
