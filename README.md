# SMBUD_project

script import Neo4j
- spotify_id
- name
- followers
- popularity
- genres
- chart_hits
          artist{spotify_id, name, followers, popularity}
          spotify_id -[:HIT{number: x}]-> country
          spotify_id -[:TYPE]-> genre
          spotify_id -[:FEAT]- spotify_id

dataset Spotify
- id spotify
- artist
- num followers (float?)
- popolarità (spike a metà)
problem:
  ---- Collaborazioni in base al genere -----
  - distribuzione features
  - feat in particolari nicchie
  - legame popolarità - num followers -> bot detection (molti num followers altissimo nonostante popolarità molto basso)
  - generi per paesi (hit per paese) -> aumentare ascolti migliorando ascolti in base al proprio paese
  - community detection

query gpt
  - artist who have collaborated across multiple genres
  - predicting future collaborations using common neighbors
  - mapping the global influence of artists
  - artists' collaboration network visualization



Dataset stipendi

MongoDB
problemi:
  - rapporti stipendio / lavoro da casa
      - are companies in certain locations more likely to offer remote work?
  - comparison by experience level and employment type
  - geographical analysis (maybe with cost of living?)
  - salary trends over time
  - company size and its impact
        - is company size influent in the remote work offer?
  - are certain jobs titles associated with significantly higher or lower salaries?
  - crescita dei vari livelli di esperienza in base al titolo
  - statistical analysis
      - which factors most strongly predict salary
      - cluster abalysis to identify distinct groups or patterns in data
  - predictive modeling
        - dimmi chi sei e ti dirò quanto guadagni (in che percentile stai )
    
queries:
  - yearly growth of average salaries in data science
      db.dataScientists.aggregate([
  { $group: {
      _id: "$work_year",
      previousYearAverage: { $avg: "$salaryinusd" }
  }},
  { $sort: { _id: 1 } },
  { $group: {
      _id: null,
      yearlyData: { $push: { year: "$_id", averageSalary: "$previousYearAverage" } }
  }},
  { $project: {
      yearlyGrowth: {
          $map: {
              input: { $range: [0, { $subtract: [{ $size: "$yearlyData" }, 1] }] },
              as: "index",
              in: {
                  year: { $arrayElemAt: ["$yearlyData.year", "$$index"] },
                  growth: {
                      $subtract: [
                          { $arrayElemAt: ["$yearlyData.averageSalary", "$$index"] },
                          { $arrayElemAt: ["$yearlyData.averageSalary", { $subtract: ["$$index", 1] }] }
                      ]
                  }
              }
          }
      }
  }}
])

  - find data scientist with salaries above the averahe in their experience level
  - most common job titles among freelancers
  - difference between different employment type with the same title
  - aziende grosse hanno dipendenti che lavorano fuori?
  - 
