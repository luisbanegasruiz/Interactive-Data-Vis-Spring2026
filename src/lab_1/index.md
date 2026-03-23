---
title: "Lab 1: Passing Pollinators"
toc: true
---

Interested in Bees? You've Come to the Right Place!


```js
const data = await FileAttachment("data/pollinator_activity_data.csv").csv({typed: true})
```

## Question 1: What is the body mass and wing span distribution of each pollinator species?
```js
display(Plot.plot({
title: "Body Mass by Pollinator Species",
width: 640,
height: 300,
marginLeft: 120,
x: { label: "Body Mass (g)", grid: true },
y: { label: null },
color: { legend: true },
marks: [
  Plot.boxX(data, {
    x: "avg_body_mass_g",
    y: "pollinator_species",
    fill: "pollinator_species",
  })
]
}))
```

```js
display(Plot.plot({
title: "Wing Span by Pollinator Species",
width: 640,
height: 300,
marginLeft: 120,
x: { label: "Wing Span (mm)", grid: true },
y: { label: null },
color: { legend: true },
marks: [
  Plot.boxX(data, {
    x: "avg_wing_span_mm",
    y: "pollinator_species",
    fill: "pollinator_species",
  })
]
}))
```
Explanation: Honeybees are the smallest species, with a body mass just above 0.1g and a wing span around 19–21mm. Bumblebees are mid-sized, weighing roughly 0.29–0.33g with wings spanning about 35–43mm. Carpenter Bees are the largest, weighing in at 0.45–0.55g with a wing span of 40–48mm. Across all three species, bigger body mass consistently comes with a bigger wing span.


## Question 2: What is the ideal weather for pollinating?
```js
display(Plot.plot({
title: "Average Visits by Weather Condition",
width: 640,
height: 300,
marginLeft: 120,
x: { label: "Average Visit Count", grid: true },
y: { label: null },
color: { legend: true },
marks: [
  Plot.barX(data, Plot.groupY(
    { x: "mean" },
    {
      x: "visit_count",
      y: "weather_condition",
      fill: "weather_condition",
      sort: { y: "-x" },
      tip: true,
    }
  )),
  Plot.ruleX([0])
]
}))
```
Explanation: Pollinators visit flowers most frequently on cloudy days, followed by partly cloudy, and least on sunny days. So contrary to what you might expect, overcast conditions seem to be ideal for pollinating activity.
## Question 3: Which flower has the most nectar production?
```js
display(Plot.plot({
title: "Average Nectar Production by Flower Species",
width: 640,
height: 300,
marginLeft: 100,
x: { label: "Nectar Production (μL)", grid: true },
y: { label: null },
color: { legend: true },
marks: [
  Plot.barX(data, Plot.groupY(
    { x: "mean" },
    {
      x: "nectar_production",
      y: "flower_species",
      fill: "flower_species",
      sort: { y: "-x" },
      tip: true,
    }
  )),
  Plot.ruleX([0])
]
}))
```
Explanation: Sunflowers produce the most nectar by a significant margin, nearly reaching 0.9 µL on average. Coneflowers come in second at around 0.6 µL, and Lavender produces the least at roughly 0.5 µL.




#Class examples on how to include the data (void)
```js
Inputs.table(data)
```
```js
Plot.plot({
marks: [
  Plot.frame(),
  Plot.barY(data, {
    x: "weather_condition",
    y: "observation_hour"
  })
]
})
```










