---
title: "Lab 1: Passing Pollinators"
toc: true
---

# Luis Banegas

```js
const data = await FileAttachment("data/pollinator_activity_data.csv").csv({typed: true})
```




## Q1: What is the body mass and wing span distribution of each pollinator species?
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




## Q2: What is the ideal weather for pollinating?
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
```js
display(Plot.plot({
title: "Average Visits by Temperature",
width: 640,
height: 300,
x: { label: "Temperature (°C)", grid: true },
y: { label: "Average Visit Count", grid: true },
marks: [
  Plot.line(data, Plot.binX(
    { y: "mean" },
    {
      x: "temperature",
      y: "visit_count",
      stroke: "#f5a623",
      strokeWidth: 2,
      thresholds: 10,
      curve: "monotone-x",
      tip: true,
    }
  )),
  Plot.ruleY([0])
]
}))
```
```js
display(Plot.plot({
title: "Average Visits by Wind Speed",
width: 640,
height: 300,
x: { label: "Wind Speed (km/h)", grid: true },
y: { label: "Average Visit Count", grid: true },
marks: [
  Plot.line(data, Plot.binX(
    { y: "mean" },
    {
      x: "wind_speed",
      y: "visit_count",
      stroke: "#5b9bd5",
      strokeWidth: 2,
      thresholds: 8,
      curve: "monotone-x",
      tip: true,
    }
  )),
  Plot.ruleY([0])
]
}))
```




## Q3: Which flower has the most nectar production?
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
```js
display(Plot.plot({
title: "Nectar Distribution by Flower Species",
width: 640,
height: 300,
marginLeft: 100,
x: { label: "Nectar Production (μL)", grid: true },
y: { label: null },
color: { legend: true },
marks: [
  Plot.boxX(data, {
    x: "nectar_production",
    y: "flower_species",
    fill: "flower_species",
    tip: true,
  })
]
}))
```












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




```js
display(Plot.plot({
title: "Average Visits by Pollinator Species",
subtitle: "Grouped by flower species",
width: 640,
height: 400,
marginLeft: 120,
x: { label: "Average Visit Count", grid: true },
y: { label: null },
color: { legend: true },
marks: [
  Plot.barX(data, Plot.groupY(
    { x: "mean" },
    {
      x: "visit_count",
      y: "pollinator_species",
      fill: "flower_species",
      tip: true,
    }
  )),
  Plot.ruleX([0])
]
}))
```







