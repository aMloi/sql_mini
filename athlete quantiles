WITH Athlete_Medals AS (
  SELECT Athlete, COUNT(*) AS Medals
  FROM Summer_Medals
  GROUP BY Athlete
  HAVING COUNT(*) > 1)
  
SELECT
  Athlete,
  Medals,
  -- Split athletes into thirds by their earned medals
  ntile(3) over(ORDER by Medals desc)AS Third
FROM Athlete_Medals
ORDER BY Medals DESC, Athlete ASC;
