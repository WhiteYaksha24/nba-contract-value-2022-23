# nba-contract-value-2022-23
Analyzing 2022-23 NBA contracts to find the best and worst value players using SQL (SQLite) and win shares / VORP per salary dollar.

NBA Contract Value 2022-23: Salary vs. Performance

Which NBA players gave their teams the most production per salary dollar in 2022-23, and which contracts were the biggest overpays?

Tools: SQL (SQLite), Excel (name cleaning). Python analysis and charts coming next. Data: Basketball Reference advanced stats (2022-23) and [YOUR SALARY SOURCE] salaries (2022-23).

Scope and limitations

This project uses the 2022-23 regular season because it is a complete season with matching public stats and salary data. Rosters and contracts have changed since, so the specific players are a demonstration of the method, which can be rerun on any season by swapping in new CSVs.

Method
Loaded the stats and salary tables into SQLite.
One row per player. Traded players appear several times in the stats file (one row per team plus a combined 2TM/3TM row). A window function (ROW_NUMBER() OVER (PARTITION BY Player ORDER BY G DESC)) keeps the row with the most games, which is the full-season total.
Name matching. Names differed between files (accents, periods, Jr./III, apostrophes). I standardized both files to plain spellings in Excel, then verified with an anti-join (LEFT JOIN ... WHERE salary IS NULL) that no player with 1,000+ minutes failed to match.
Value metrics:
WS per $1M = Win Shares / (salary / 1,000,000)
VORP per $1M = VORP / (salary / 1,000,000)
Filters: at least 1,000 minutes played, to keep out small-sample noise.
"Star" definition: salary of at least $20M and at least 1,000 minutes. Without this split, the best-value lists are dominated by rookie-scale contracts, so stars need their own comparison.

I used both Win Shares and VORP because they are built differently (VORP measures value over a replacement-level player; WS is built from box-score production and team context). Players who rank well on both are the most defensible picks.

Findings
Stars ($20M+ salary): best value
Metric	#1	#2
VORP per $1M	Domantas Sabonis, 0.256 ($21.1M)	Joel Embiid ($33.6M)
WS per $1M	Domantas Sabonis, 0.597	[confirm from your results]

Joel Embiid, the 2022-23 MVP, posted 0.366 WS per $1M on his $33.6M salary.

Sabonis, on a $21.1M salary, out-produced the 2022-23 MVP (Embiid) per dollar on VORP and was #1 on WS per dollar as well.
On both top-10 lists (best value on both metrics): Shai Gilgeous-Alexander, Jayson Tatum, Joel Embiid, Mikal Bridges, Julius Randle, Jimmy Butler.
Top 10 WS per $1M only: Aaron Gordon, Jalen Brunson, Jarrett Allen.
Top 10 VORP per $1M only: Donovan Mitchell, Giannis Antetokounmpo, Fred VanVleet.
Stars ($20M+ salary): worst value
Lowest VORP per $1M: Gordon Hayward, 0.007 on a $30.07M salary.
Russell Westbrook appears twice in the worst lists because both his Lakers row (52 games) and his full-season row (73 games) qualified. His Clippers stint (21 games) did not meet the minutes filter.
Lakers: 0.015 WS per $1M and 0.015 VORP per $1M
Full season: 0.04 WS per $1M and 0.025 VORP per $1M (salary $47.08M)
On both bottom-10 lists (worst value on both metrics): Russell Westbrook, Gordon Hayward, Andrew Wiggins, Klay Thompson, Ben Simmons, Kevin Love.
Bottom 10 WS per $1M only: Terry Rozier, Bradley Beal, Paul George.
Bottom 10 VORP per $1M only: John Collins, Anfernee Simons, Mike Conley.
All players: best value
Highest VORP per $1M: Desmond Bane, 1.174 ($2.13M), then Tyrese Haliburton, 1.044 ($4.21M).
Highest WS per $1M: Austin Reaves ($1.56M) and Anthony Lamb ($6.94M) tied at 3.454. (see data notes)
On both top-10 lists: Desmond Bane, Austin Reaves, Walker Kessler, Daniel Gafford, Immanuel Quickley.
Top 10 VORP per $1M only: Jordan Goodwin, Sam Hauser, Isaiah Joe, Naz Reid.
Top 10 WS per $1M only: Anthony Lamb, Nick Richards, Drew Eubanks, Terance Mann, Quentin Grimes.

Haliburton ranking near the top of the value list is consistent with what followed: he was named an All-Star that season.

All players: worst value
Lowest VORP per $1M: Malachi Branham, -0.444 ($2.95M), then Gabe Vincent, -0.33 ($1.8M).
Lowest WS per $1M: Killian Hayes, -0.086 ($5.84M), then Malachi Branham, -0.034.
On both bottom-10 lists: Malachi Branham, Killian Hayes, Jeremy Sochan.
Bottom 10 WS per $1M only: Russell Westbrook (two rows), Reggie Jackson (two rows: 52 games with the Clippers, 68 overall; his Nuggets stint did not qualify), Jaden Ivey, Eric Gordon, Ben Simmons.
Bottom 10 VORP per $1M only: Gabe Vincent, Andrew Nembhard, David Roddy, Tre Mann, Oshae Brissett, Bennedict Mathurin, Ayo Dosunmu.
Takeaways
The best-value contracts were almost all cheap deals. The all-player top 10s are dominated by players earning under roughly $7M, so ratios reward low salaries. That is why the $20M star filter matters.
Among stars, the best value came from players whose salary was lower than their impact. Sabonis and the MVP ranked above higher-paid stars on both metrics.
The two metrics disagree often enough to matter. Only 6 of 10 top stars appear on both lists, so using one metric alone would change the conclusions. Players on both are the most robust picks.
Big contracts for declining or injured players were the worst value. Westbrook, Hayward, Simmons, Wiggins, Thompson, and Love all appear near the bottom on both metrics.
The worst values among low-salary players were young players playing heavy minutes (Branham, Hayes, Sochan), a different kind of "bad value": cheap, but on the court a lot with low production.
If I were the GM: targets

My targets are Desmond Bane, Austin Reaves, and Walker Kessler. All three ranked in the top 10 on both VORP per $1M and WS per $1M among all players, so the numbers support them on two different metrics. They are complementary role players rather than full-blown stars, which fits a team that wants to add depth and pieces without paying star prices.

Desmond Bane (sharpshooter). Highest VORP per $1M in the league (1.174 on a $2.13M salary). He would be the spacing piece: a shooter who produces far beyond his contract.
Austin Reaves (shot creator). Top of the league in WS per $1M on a $1.56M salary. He creates his own shot and is very good at drawing fouls, which adds efficient offense. His price in a trade would be high, but his age and potential would justify it.
Walker Kessler (defensive anchor). On both top-10 lists. He would anchor the defense at the rim, which is the area where box-score value metrics tend to understate a player's impact.

Why not Sabonis? He was the best value among stars (0.256 VORP per $1M, 0.597 WS per $1M), but he would likely cost too much in a trade, so I targeted cheaper players with similar value per dollar.

Together they cover three different needs (shooting, shot creation, defense) while staying cheap relative to their production. 

Data notes and limitations
Traded players: players with multiple team stints appear as separate rows in some results (Westbrook, Reggie Jackson). The window function keeps only the full-season row in the cleaned view, so this needs a recheck against the view being queried. [Confirm and update this note.]
Ratios favor cheap contracts and can behave oddly for low-minute players (VORP can be negative), which the 1,000-minute filter reduces but doesn't remove.
WS and VORP are box-score based and don't fully capture defense, film, injuries, or fit.
A single season is a small sample. A next step is to check whether the best-value players kept producing the following year.
Repo structure
data/        raw CSVs
sql/         queries used in this analysis
README.md
How to run
Load the two CSVs into SQLite (table names stats and salaries).
Run the queries in sql/ in order: create the one-row-per-player view, check name matches, then run the value queries.
