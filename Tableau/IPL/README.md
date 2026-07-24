# IPL Statistical Analysis Dashboard (2008-2025)

Interactive Tableau dashboard analyzing 17 seasons of IPL data: team performance, 
bowler statistics, and season-wise trends, with careful handling of rain-affected 
and incomplete matches.

## What this covers
- Season-wise team and batting statistics
- Purple Cap (bowler wickets) analysis
- Highest & lowest team totals per season
- Champions summary: title winners and championship counts by team
- Interactive filters for season

## Key challenges solved

**1. Match Completeness Logic**
Many IPL matches are affected by D/L method, rain interruptions, or are abandoned 
midway. To keep the "minimum team score" analysis accurate, I only counted an 
innings as valid if the team played all 20 overs or lost all 10 wickets.
```
IF { FIXED [Match Id]: SUM([Wicket_count]) } = 10
OR { FIXED [Match Id]: MAX([Over Number]) } >= 19
THEN "Complete"
ELSE "Incomplete"
END
```
*(Overs stored as 0-19 in this dataset, so 19 = a full 20-over innings)*

Learned the hard way that this filter needs to be **added to context**, otherwise 
it doesn't get applied before other calculations run, which silently breaks the logic.

**2. Accurate Bowler Wicket Count (Purple Cap)**
Not every wicket counts toward a bowler's tally. Run outs, retired hurt, and 
retired out shouldn't be credited to the bowler.

```
IF [Wicket Kind] != "run out"
AND [Wicket Kind] != "retired hurt"
AND [Wicket Kind] != "retired out"
THEN 1
ELSE 0
END
```

## Tools
Tableau, LOD (Level of Detail) expressions, Calculated fields

## Dataset
Ball-by-ball IPL data (2008-2025), sourced from a public Kaggle dataset.

## Dashboard preview
![IPL Dashboard](IPL_Statistical_Analysis.jfif)

## Video walkthrough
🎥 [Watch dashboard walkthrough](https://github.com/user-attachments/assets/e965629f-da54-402b-9704-e6780c1b1843)

## What I learned
- Filter context and why order of operations matters in Tableau
- Data validation mindset for messy real-world sports data
- Handling edge cases (incomplete innings, non-standard dismissals)
- Debugging patience: the "forgot to add to context" bug took about 30 minutes to trace
