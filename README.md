# Database-project

1. Project Overview & Purpose
This database is designed to provide a structured way to manage a sports team's operations, including tracking players, coaches, game schedules, attendance, and player performance statistics. It replaces disorganized spreadsheets or paper-based tracking by centralizing all key information into a single, accessible system.
Stakeholders
Team management – Needs accurate player and game data for decision-making.


Coaches – Use the database to track training progress and performance.


Players – Can review their performance statistics.


Fans & sponsors – Indirect beneficiaries through better team organization.
2. Design Decisions & Challenges
Normalization – Ensured that all tables met 3NF requirements to avoid redundancy and anomalies.


Relationship Mapping – Carefully determined one-to-many and many-to-many relationships (e.g., a player can attend many games, and a game can have many players).


Query Optimization – Choose indexing strategies for faster searches on player names and game dates.


Challenges Faced – Designing foreign keys for attendance and performance tables without creating circular dependencies.
3. Normalization Analysis
First Normal Form (1NF)
Requirement: All attributes must be atomic, no repeating groups, and unique rows.


Compliance:


Each table has a primary key.


All columns hold atomic values (no multi-valued attributes like “skills = dribbling, passing” in one column).


Example: Instead of storing multiple player positions in one cell, we store each in a separate row.
Second Normal Form (2NF)
Requirement: Meet 1NF and remove partial dependencies (non-key attributes depending on part of a composite key).


Compliance:


In the Attendance table (with composite PK: PlayerID, GameID), no non-key attribute depends on just one part of the composite key.


Player name or team name is not stored here — they are retrieved via relationships to the Players and Games tables.
Third Normal Form (3NF)
Requirement: Meet 2NF and remove transitive dependencies (non-key attributes depending on other non-key attributes).


Compliance:


In the Players table, attributes like Position and JerseyNumber depend only on PlayerID (the key), not on other attributes.


No derived data (e.g., total games played) is stored — it is calculated through queries.
4. Relationship Verification
Players ↔ Attendance: One player can attend many games; each attendance record links one player to one game.


Games ↔ Attendance: One game can have many players; each attendance record links one game to one player.


Players ↔ PerformanceStats: One player can have many performance stat entries (one per game).


Coaches ↔ Teams: One coach belongs to exactly one team; a team can have many coaches.
5. Real-World Scenarios & Example Queries
Scenario 1: Retrieve all games a specific player has attended
SELECT g.GameID, g.GameDate, g.Location
FROM Attendance a
JOIN Games g ON a.GameID = g.GameID
JOIN Players p ON a.PlayerID = p.PlayerID
WHERE p.PlayerName = 'John Doe';

Scenario 2: Find the top 5 players by average points per game
SELECT p.PlayerName, AVG(ps.Points) AS AvgPoints
FROM PerformanceStats ps
JOIN Players p ON ps.PlayerID = p.PlayerID
GROUP BY p.PlayerName
ORDER BY AvgPoints DESC
LIMIT 5;

Scenario 3: List all coaches for a given team
SELECT c.CoachName
FROM Coaches c
JOIN Teams t ON c.TeamID = t.TeamID
WHERE t.TeamName = 'Lions';
