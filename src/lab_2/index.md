---
title: "Lab 2: Subway Staffing"
toc: true
---

```js
const incidents = FileAttachment("./data/incidents.csv").csv({ typed: true })
const local_events = FileAttachment("./data/local_events.csv").csv({ typed: true })
const upcoming_events = FileAttachment("./data/upcoming_events.csv").csv({ typed: true })
const ridership = FileAttachment("./data/ridership.csv").csv({ typed: true })
```

```js
const currentStaffing = {
  "Times Sq-42 St": 19,
  "Grand Central-42 St": 18,
  "34 St-Penn Station": 15,
  "14 St-Union Sq": 4,
  "Fulton St": 17,
  "42 St-Port Authority": 14,
  "Herald Sq-34 St": 15,
  "Canal St": 4,
  "59 St-Columbus Circle": 6,
  "125 St": 7,
  "96 St": 19,
  "86 St": 19,
  "72 St": 10,
  "66 St-Lincoln Center": 15,
  "50 St": 20,
  "28 St": 13,
  "23 St": 8,
  "Christopher St": 15,
  "Houston St": 18,
  "Spring St": 12,
  "Chambers St": 18,
  "Wall St": 9,
  "Bowling Green": 6,
  "West 4 St-Wash Sq": 4,
  "Astor Pl": 7
}
```

---

## 1. How did local events impact ridership in summer 2025? What effect did the July 15th fare increase have?

The chart below shows daily ridership across all 25 Manhattan stations in summer 2025. After the July 15th fare increase ($2.75 → $3.00), marked by the dashed red line, ridership drops noticeably and remains lower through August. The before/after comparison confirms this — average daily ridership was higher in the weeks before the fare change.

```js
const eventDates = new Set(local_events.map(d => d.date))

const ridershipByDate = {}
for (const row of ridership) {
  if (!ridershipByDate[row.date]) {
    ridershipByDate[row.date] = { total: 0, has_event: eventDates.has(row.date) }
  }
  ridershipByDate[row.date].total += row.entrances + row.exits
}

const dailyRidership = Object.entries(ridershipByDate)
  .map(([date, vals]) => ({ date: new Date(date), ...vals }))
  .sort((a, b) => a.date - b.date)
```

```js
Plot.plot({
  title: "Daily Ridership — Summer 2025",
  width: 800,
  marginLeft: 80,
  x: { type: "time", label: "Date" },
  y: { label: "Total Ridership", zero: true },
  marks: [
    Plot.lineY(dailyRidership, {
      x: "date",
      y: "total",
      stroke: "steelblue",
      strokeWidth: 2
    }),
    Plot.dot(dailyRidership.filter(d => d.has_event), {
      x: "date",
      y: "total",
      fill: "orange",
      stroke: "white",
      strokeWidth: 1,
      r: 5,
      tip: true,
      title: d => `Event day\n${d.total.toLocaleString()} riders`
    }),
    Plot.ruleX([new Date("2025-07-15")], {
      stroke: "red",
      strokeDasharray: "6,4",
      strokeWidth: 2
    }),
    Plot.text(
      [{ date: new Date("2025-07-15"), total: dailyRidership.reduce((max, d) => d.total > max ? d.total : max, 0) }],
      {
        x: "date",
        y: "total",
        text: () => "Fare ↑ $3.00",
        dx: 6,
        dy: -10,
        fill: "red",
        fontSize: 11,
        fontWeight: "bold",
        textAnchor: "start"
      }
    )
  ]
})
```

```js
const beforeFare = dailyRidership.filter(d => d.date < new Date("2025-07-15"))
const afterFare = dailyRidership.filter(d => d.date >= new Date("2025-07-15"))

const fareCompare = [
  { period: "Before Jul 15", avg: beforeFare.reduce((s, d) => s + d.total, 0) / beforeFare.length },
  { period: "After Jul 15", avg: afterFare.reduce((s, d) => s + d.total, 0) / afterFare.length }
]
```

```js
Plot.plot({
  title: "Average Daily Ridership Before vs. After Fare Increase",
  width: 500,
  x: { label: "Avg. Daily Riders" },
  y: { label: null },
  marks: [
    Plot.barX(fareCompare, {
      x: "avg",
      y: "period",
      fill: d => d.period.startsWith("Before") ? "steelblue" : "tomato",
      tip: true
    }),
    Plot.ruleX([0])
  ]
})
```

---

## 2. How do the stations compare when it comes to response time? Which are the best, which are the worst?

The chart below ranks all 25 stations by mean incident response time. The worst performers — 59 St-Columbus Circle, West 4 St-Wash Sq, and 125 St — all have fewer than 7 staff members. The best performers like Fulton St, Houston St, and Times Sq-42 St are all well-staffed with 17 or more. The pattern is clear: more staff means faster response times.

```js
const responseByStation = Array.from(
  Map.groupBy(incidents, d => d.station),
  ([station, rows]) => ({
    station,
    mean_response: rows.reduce((s, d) => s + d.response_time_minutes, 0) / rows.length,
    staffing: currentStaffing[station] ?? 0
  })
).sort((a, b) => b.mean_response - a.mean_response)

const sortedTimes = [...incidents].map(d => d.response_time_minutes).sort((a, b) => a - b)
const mid = Math.floor(sortedTimes.length / 2)
const medianResponse = sortedTimes.length % 2
  ? sortedTimes[mid]
  : (sortedTimes[mid - 1] + sortedTimes[mid]) / 2
```

```js
Plot.plot({
  title: "Mean Incident Response Time by Station",
  subtitle: "Dot color = current staffing level · dashed line = system median",
  width: 800,
  height: 500,
  marginLeft: 200,
  x: { label: "Mean Response Time (minutes)", domain: [0, 20] },
  y: { label: null, domain: responseByStation.map(d => d.station) },
  color: {
    label: "Staff Count",
    scheme: "RdYlGn",
    domain: [3, 20],
    legend: true
  },
  marks: [
    Plot.ruleY(responseByStation, {
      y: "station",
      x1: 0,
      x2: "mean_response",
      stroke: "#ccc"
    }),
    Plot.dot(responseByStation, {
      y: "station",
      x: "mean_response",
      r: 8,
      fill: "staffing",
      tip: true,
      title: d => `${d.station}\nMean RT: ${d.mean_response.toFixed(1)} min\nStaff: ${d.staffing}`
    }),
    Plot.text(responseByStation, {
      y: "station",
      x: "mean_response",
      text: d => String(d.staffing),
      fill: "white",
      fontSize: 9,
      fontWeight: "bold"
    }),
    Plot.ruleX([medianResponse], {
      stroke: "navy",
      strokeDasharray: "6,3",
      strokeWidth: 2
    }),
    Plot.text(
      [{ x: medianResponse, y: responseByStation[0].station }],
      {
        x: "x",
        y: "y",
        text: () => `Median: ${medianResponse.toFixed(1)} min`,
        dy: -12,
        fill: "navy",
        fontSize: 11,
        fontWeight: "bold"
      }
    )
  ]
})
```

---

## 3. Which three stations need the most staffing help for next summer based on the 2026 event calendar?

The chart below ranks stations by expected 2026 attendance divided by current staffing. Canal St stands out by a wide margin — it has very few staff but high expected attendance. 34 St-Penn Station and 23 St round out the top three. These three stations will be the most stretched heading into summer 2026 and should be prioritized for additional staffing.

```js
const load2026 = Array.from(
  Map.groupBy(upcoming_events, d => d.nearby_station),
  ([station, rows]) => ({
    station,
    total_expected: rows.reduce((s, d) => s + d.expected_attendance, 0),
    event_count: rows.length,
    staffing: currentStaffing[station] ?? 1,
    load_per_staff: rows.reduce((s, d) => s + d.expected_attendance, 0) / (currentStaffing[station] ?? 1)
  })
).sort((a, b) => b.load_per_staff - a.load_per_staff)

const top3 = load2026.slice(0, 3)
```

```js
Plot.plot({
  title: "2026 Event Load Per Staff Member by Station",
  subtitle: "Top 3 most at-risk stations flagged in red",
  width: 800,
  height: 480,
  marginLeft: 200,
  x: { label: "Expected Attendance ÷ Current Staff" },
  y: { label: null, domain: load2026.map(d => d.station) },
  marks: [
    Plot.barX(
      upcoming_events,
      Plot.groupY(
        { x: "sum" },
        {
          y: "nearby_station",
          x: "expected_attendance",
          fill: d => top3.find(t => t.station === d.nearby_station) ? "tomato" : "steelblue",
          sort: { y: "-x" },
          tip: true
        }
      )
    ),
    Plot.text(top3, {
      x: 0,
      y: "station",
      text: (d, i) => `#${i + 1}`,
      dx: -8,
      textAnchor: "end",
      fill: "tomato",
      fontSize: 11,
      fontWeight: "bold"
    }),
    Plot.ruleX([0])
  ]
})
```

## 4. [BONUS] If you had to prioritize one station to get increased staffing, which would it be and why?

The heatmap below scores each station across three risk dimensions. The darker the cell, the higher the risk. The station with the most dark cells across all three dimensions is the top priority.

```js
const bonusScores = responseByStation.map(d => {
  const ev = load2026.find(e => e.station === d.station)
  const rtRank = responseByStation.findIndex(r => r.station === d.station) / responseByStation.length
  const evRank = ev ? load2026.findIndex(e => e.station === d.station) / load2026.length : 0.5
  const staffRisk = 1 - (d.staffing / 20)
  return {
    station: d.station,
    staffing: d.staffing,
    composite: (rtRank + evRank + staffRisk) / 3,
    rt_rank: rtRank,
    ev_rank: evRank,
    staff_risk: staffRisk
  }
}).sort((a, b) => b.composite - a.composite)

const winner = bonusScores[0]

const heatRows = bonusScores.flatMap(d => [
  { station: d.station, dimension: "Response Time", value: d.rt_rank },
  { station: d.station, dimension: "2026 Event Load", value: d.ev_rank },
  { station: d.station, dimension: "Low Staffing", value: d.staff_risk }
])
```

```js
Plot.plot({
  title: "Station Risk Heatmap",
  subtitle: "Darker = higher risk · stations sorted worst to best · top priority labeled",
  width: 600,
  height: 560,
  marginLeft: 200,
  marginBottom: 60,
  x: { label: null },
  y: { label: null, domain: bonusScores.map(d => d.station) },
  color: { scheme: "YlOrRd", domain: [0, 1], legend: true, label: "Risk Score" },
  marks: [
    Plot.cell(heatRows, {
      x: "dimension",
      y: "station",
      fill: "value",
      inset: 1,
      tip: true,
      title: d => `${d.station}\n${d.dimension}: ${(d.value * 100).toFixed(0)}th percentile`
    }),
    Plot.text(
      heatRows.filter(d => d.station === winner.station && d.dimension === "Response Time"),
      {
        x: "dimension",
        y: "station",
        text: () => "◄ Top Priority",
        dx: -4,
        textAnchor: "end",
        fill: "red",
        fontSize: 10,
        fontWeight: "bold"
      }
    )
  ]
})
```