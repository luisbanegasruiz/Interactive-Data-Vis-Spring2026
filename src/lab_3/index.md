---
title: "Lab 3: Mayoral Mystery"
toc: true
---

# Mayoral Mystery
### by Luis E. Banegas 

```js
const election = await FileAttachment("./data/election_results.csv").csv({ typed: true })
const events = await FileAttachment("./data/campaign_events.csv").csv({ typed: true })
const survey = await FileAttachment("./data/survey_responses.csv").csv({ typed: true })
const nyc = await FileAttachment("./data/nyc.json").json()
```

```js
import * as topojson from "npm:topojson-client"
```

```js
const districts = topojson.feature(nyc, nyc.objects.districts)
```

```js
const electionEnriched = election.map(d => ({
  ...d,
  vote_share: d.votes_candidate / (d.votes_candidate + d.votes_opponent),
  won: d.votes_candidate > d.votes_opponent,
  borough: d.boro_cd < 200 ? "Manhattan"
         : d.boro_cd < 300 ? "Bronx"
         : d.boro_cd < 400 ? "Brooklyn"
         : d.boro_cd < 500 ? "Queens"
         : "Staten Island"
}))

const voteShareByDistrict = new Map(electionEnriched.map(d => [d.boro_cd, d.vote_share]))

districts.features.forEach(f => {
  f.properties.vote_share = voteShareByDistrict.get(f.properties.BoroCD) ?? null
})
```

```js
const policies = [
  { key: "affordable_housing_alignment", label: "Affordable housing" },
  { key: "public_transit_alignment",     label: "Public transit" },
  { key: "childcare_support_alignment",  label: "Childcare support" },
  { key: "small_business_tax_alignment", label: "Small business tax" },
  { key: "police_reform_alignment",      label: "Police reform" }
]

const policyRows = survey.flatMap(d => {
  const group = d.voted_for === "Candidate" ? "Voted candidate"
              : d.voted_for === "Opponent"  ? "Voted opponent"
              : "Did not vote / no answer"
  return policies.map(p => ({
    label: p.label,
    score: +d[p.key],
    group
  }))
})
```

---

## 1. Where did the candidate win and lose?

The map below shows the candidate's vote share across every NYC community district. Blue districts leaned toward the candidate; red districts leaned toward the opponent. Yellow dots show campaign events — larger dots indicate higher attendance. The candidate's strength is concentrated in low-income Bronx and Brooklyn districts. High-income districts in Manhattan, northeast Queens, and across Staten Island went decisively to the opponent.

```js
Plot.plot({
  title: "Candidate Vote Share by NYC Community District",
  subtitle: "Blue = candidate stronger · Red = opponent stronger · Dots = campaign events sized by attendance",
  width: 800,
  height: 560,
  projection: { type: "mercator", domain: districts },
  color: {
    type: "linear",
    domain: [0.3, 0.5, 0.7],
    range: ["#c0392b", "#f5f0e8", "#2980b9"],
    label: "Candidate vote share",
    legend: true,
    tickFormat: d => `${(d * 100).toFixed(0)}%`
  },
  marks: [
    Plot.geo(districts, {
      fill: f => f.properties.vote_share,
      stroke: "white",
      strokeWidth: 0.5,
      tip: true,
      title: f =>
        `District ${f.properties.BoroCD}\n` +
        (f.properties.vote_share != null
          ? `Vote share: ${(f.properties.vote_share * 100).toFixed(1)}%`
          : "No data")
    }),
    Plot.dot(events, {
      x: "longitude",
      y: "latitude",
      r: d => Math.sqrt(d.estimated_attendance) * 0.9,
      fill: "yellow",
      fillOpacity: 0.7,
      stroke: "black",
      strokeWidth: 0.5,
      tip: true,
      title: d => `${d.event_type}\nAttendance: ${d.estimated_attendance}\n${d.event_date}`
    })
  ]
})
```

---

## 2. How did results vary by income level?

The chart below shows the candidate's vote share in each district, grouped by income category. The income divide is stark: the candidate won nearly every low-income district but lost almost all high-income ones. Middle-income districts split almost evenly — that is where the election was decided. Winning even a few more of those would have flipped the outcome.

```js
Plot.plot({
  title: "Candidate Vote Share by District, Grouped by Income Level",
  subtitle: "Each dot is one community district · dashed line = 50% threshold",
  width: 720,
  height: 300,
  marginLeft: 90,
  x: {
    label: "Candidate vote share →",
    tickFormat: d => `${(d * 100).toFixed(0)}%`,
    domain: [0.2, 0.8]
  },
  y: { label: null, domain: ["Low", "Middle", "High"] },
  color: {
    domain: ["Low", "Middle", "High"],
    range: ["#2ecc71", "#3498db", "#e67e22"],
    legend: true,
    label: "Income category"
  },
  marks: [
    Plot.dot(electionEnriched, {
      x: "vote_share",
      y: "income_category",
      fill: "income_category",
      r: 6,
      fillOpacity: 0.75,
      tip: true,
      title: d => `District ${d.boro_cd} (${d.borough})\nVote share: ${(d.vote_share * 100).toFixed(1)}%\nIncome: ${d.income_category}`
    }),
    Plot.ruleX([0.5], { stroke: "#999", strokeDasharray: "6,3", strokeWidth: 1.5 })
  ]
})
```

---

## 3. How did people feel about the issues?

Affordable housing and public transit earn the highest alignment scores even among opponent voters — these are broadly popular positions that the candidate should lead with. Police reform stands out as the weakest issue by far: opponent voters rated it around 1.5 out of 5, meaning it is actively alienating potential supporters. This is the single biggest messaging liability in the campaign. The gap on small business tax between candidate voters and everyone else suggests it is polarizing and should be reframed or deprioritized.

```js
Plot.plot({
  title: "Average Policy Alignment Score by Issue",
  subtitle: "Survey respondents rated each issue 1–5 · split by voting behavior",
  width: 720,
  height: 320,
  marginLeft: 140,
  x: {
    label: "Average alignment score (1–5) →",
    domain: [1, 5]
  },
  y: { label: null },
  color: {
    domain: ["Voted candidate", "Voted opponent", "Did not vote / no answer"],
    range: ["#2980b9", "#c0392b", "#95a5a6"],
    legend: true
  },
  marks: [
    Plot.dot(
      policyRows,
      Plot.groupY(
        { x: "mean" },
        {
          y: "label",
          x: "score",
          fill: "group",
          stroke: "white",
          strokeWidth: 0.5,
          r: 7,
          tip: true,
          sort: { y: "-x" }
        }
      )
    ),
    Plot.ruleX([3], { stroke: "#ccc", strokeDasharray: "4,3" })
  ]
})
```

---

## 4. Was the Get Out The Vote campaign effective?

More doors knocked correlates with higher vote share — but almost all the heavy canvassing landed in low-income districts the candidate was already winning. Middle-income swing districts received far fewer doors knocked despite being the most competitive. The effort was well-intentioned but mis-targeted.

```js
Plot.plot({
  title: "GOTV Doors Knocked vs. Candidate Vote Share",
  subtitle: "Each dot = one district · colored by income level · dashed line = trend",
  width: 720,
  height: 400,
  marginLeft: 60,
  x: {
    label: "Doors knocked →",
    tickFormat: d => d.toLocaleString()
  },
  y: {
    label: "↑ Candidate vote share",
    tickFormat: d => `${(d * 100).toFixed(0)}%`,
    domain: [0.2, 0.8]
  },
  color: {
    domain: ["Low", "Middle", "High"],
    range: ["#2ecc71", "#3498db", "#e67e22"],
    legend: true,
    label: "Income category"
  },
  marks: [
    Plot.linearRegressionY(electionEnriched, {
      x: "gotv_doors_knocked",
      y: "vote_share",
      stroke: "#333",
      strokeWidth: 1.5,
      strokeDasharray: "3,3"
    }),
    Plot.dot(electionEnriched, {
      x: "gotv_doors_knocked",
      y: "vote_share",
      fill: "income_category",
      r: 6,
      fillOpacity: 0.8,
      tip: true,
      title: d =>
        `District ${d.boro_cd} (${d.borough})\n` +
        `Doors knocked: ${d.gotv_doors_knocked.toLocaleString()}\n` +
        `Vote share: ${(d.vote_share * 100).toFixed(1)}%\n` +
        `Income: ${d.income_category}`
    }),
    Plot.ruleY([0.5], { stroke: "#999", strokeDasharray: "6,3", strokeWidth: 1.5 })
  ]
})
```

---

## Recommendations for the next run

**1. Focus canvassing on middle-income swing districts.** Redirecting GOTV effort toward middle-income districts in Queens and Brooklyn — where margins were razor thin — could flip enough districts to change the outcome.

**2. Lead with housing and transit, pull back on police reform.** Housing and transit score above 3.5 even among opponent voters. Police reform scores lowest across every group and likely costs more votes than it wins.

**3. Invest candidate time in high-income districts.** Personal time was lowest in the districts lost by the largest margins. Even modest investment there — especially on transit and small business tax — could convert enough persuadable voters to matter.