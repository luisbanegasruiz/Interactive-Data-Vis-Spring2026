# Invisible Targets

<style>
  @import url('https://fonts.googleapis.com/css2?family=Playfair+Display:ital,wght@0,700;0,900;1,400&family=Source+Serif+4:ital,opsz,wght@0,8..60,400;0,8..60,600;1,8..60,400&family=Source+Sans+3:wght@400;600&display=swap');

  :root {
    --ink: #111010;
    --gray: #6b6b6b;
    --light-gray: #f5f4f0;
    --rule: #d0cfc9;
    --accent: #8b1a1a;
    --max-width: 720px;
  }

  #observablehq-main h1,
  #observablehq-main h2,
  #observablehq-main h3,
  #observablehq-main p {
    all: unset;
    display: block;
  }

  h1 {
    font-family: 'Playfair Display', Georgia, serif !important;
    font-size: 1rem !important;
    color: var(--ink) !important;
  }

  .hero {
    position: relative;
    width: 100vw;
    left: 50%;
    right: 50%;
    margin-left: -50vw;
    margin-right: -50vw;
    background: #1a1a1a;
    min-height: 92vh;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: flex-end;
    padding-bottom: 5rem;
    overflow: hidden;
    margin-bottom: 4rem;
  }

  .hero-map {
    position: absolute;
    inset: 0;
    width: 100%;
    height: 100%;
    opacity: 0.55;
  }

  .hero-map svg {
    width: 100% !important;
    height: 100% !important;
  }

  .hero-text {
    position: relative;
    z-index: 10;
    text-align: center;
    padding: 0 2rem;
    max-width: 900px;
  }

  .hero-kicker {
    font-family: 'Source Sans 3', sans-serif;
    font-size: 0.78rem;
    font-weight: 600;
    letter-spacing: 0.18em;
    text-transform: uppercase;
    color: #c8a97e;
    margin-bottom: 1.2rem;
  }

  .hero-title {
    font-family: 'Playfair Display', Georgia, serif;
    font-size: clamp(3.5rem, 8vw, 7rem);
    font-weight: 900;
    color: #fff;
    line-height: 1.0;
    letter-spacing: -0.01em;
    margin-bottom: 1.4rem;
  }

  .hero-subtitle {
    font-family: 'Source Serif 4', Georgia, serif;
    font-size: clamp(1rem, 2vw, 1.25rem);
    color: #c8c4bc;
    line-height: 1.6;
    margin-bottom: 2rem;
    font-style: italic;
  }

  .hero-byline {
    font-family: 'Source Sans 3', sans-serif;
    font-size: 0.88rem;
    color: #a0998f;
    letter-spacing: 0.04em;
  }

  .hero-byline strong {
    color: #d4cec6;
    font-weight: 600;
  }

  .article-body {
    max-width: var(--max-width);
    margin: 0 auto;
    padding: 0 1.5rem;
  }

  .drop-cap p:first-child::first-letter {
    font-family: 'Playfair Display', Georgia, serif;
    font-size: 4.2rem;
    font-weight: 900;
    float: left;
    line-height: 0.78;
    margin: 0.08em 0.1em 0 0;
    color: var(--accent);
  }

  .article-body p {
    font-family: 'Source Serif 4', Georgia, serif !important;
    font-size: 1.08rem !important;
    line-height: 1.95 !important;
    color: #1a1a1a !important;
    margin-bottom: 1.4rem !important;
    max-width: var(--max-width) !important;
  }

  .section-label {
    font-family: 'Source Sans 3', sans-serif;
    font-size: 0.68rem;
    font-weight: 600;
    letter-spacing: 0.22em;
    text-transform: uppercase;
    color: var(--ink);
    max-width: var(--max-width);
    margin: 3rem auto 0.6rem;
    padding: 0 1.5rem;
    border-top: 2px solid var(--ink);
    padding-top: 0.6rem;
  }

  .stat-block {
    background: var(--ink);
    color: #fff;
    padding: 3rem 3.5rem;
    margin: 3rem auto;
    max-width: var(--max-width);
  }

  .stat-block .number {
    font-family: 'Playfair Display', Georgia, serif;
    font-size: 5rem;
    font-weight: 900;
    color: #c8a97e;
    line-height: 1;
    display: block;
  }

  .stat-block .label {
    font-family: 'Source Serif 4', Georgia, serif;
    font-size: 1.1rem;
    color: #c8c4bc;
    line-height: 1.7;
    margin-top: 0.8rem;
    display: block;
    font-style: italic;
  }

  .chart-caption {
    font-family: 'Source Sans 3', sans-serif;
    font-size: 0.78rem;
    color: var(--gray);
    max-width: var(--max-width);
    margin: 0.5rem auto 2.5rem;
    padding: 0 1.5rem;
    line-height: 1.6;
  }

  .closing {
    background: var(--ink);
    padding: 2.5rem 3rem;
    margin: 3rem auto;
    max-width: var(--max-width);
  }

  .closing p {
    font-family: 'Source Serif 4', Georgia, serif !important;
    font-size: 1.05rem !important;
    line-height: 1.9 !important;
    color: #c8c4bc !important;
    margin-bottom: 0 !important;
  }

  .footnote {
    font-family: 'Source Sans 3', sans-serif;
    font-size: 0.75rem;
    color: var(--gray);
    max-width: var(--max-width);
    margin: 2rem auto 4rem;
    padding: 0 1.5rem;
    border-top: 1px solid var(--rule);
    padding-top: 1rem;
  }
</style>

```js
import { feature } from "npm:topojson-client";
```

```js
const data = await FileAttachment("data/arrestsdata.csv").csv({ typed: true });
const today = new Date();
const cleanData = data.filter(d => d.apprehension_date <= today);
const genderCounts = d3.rollup(cleanData, v => v.length, d => d.gender);
const totalCount = cleanData.length;
const maleCount = genderCounts.get("Male");
const malePct = ((maleCount / totalCount) * 100).toFixed(1);
```

```js
const world = await fetch("https://cdn.jsdelivr.net/npm/world-atlas@2/countries-110m.json").then(r => r.json());
const countries110 = feature(world, world.objects.countries);

const allCountryNames = Array.from(
  d3.rollup(cleanData, v => v.length, d => d.citizenship_country),
  ([country, count]) => ({ country, count })
);

const allGenderByCountry = new Map(
  Array.from(
    d3.rollup(cleanData, v => v.length, d => d.citizenship_country, d => d.gender)
  )
);

const fullCountryMap = new Map([
  ["484", "MEXICO"], ["320", "GUATEMALA"], ["340", "HONDURAS"], ["862", "VENEZUELA"],
  ["558", "NICARAGUA"], ["170", "COLOMBIA"], ["222", "EL SALVADOR"], ["192", "CUBA"],
  ["218", "ECUADOR"], ["604", "PERU"], ["214", "DOMINICAN REPUBLIC"], ["076", "BRAZIL"],
  ["356", "INDIA"], ["156", "CHINA, PEOPLES REPUBLIC OF"], ["332", "HAITI"], ["388", "JAMAICA"],
  ["704", "VIETNAM"], ["642", "ROMANIA"], ["643", "RUSSIA"], ["792", "TURKIYE"],
  ["152", "CHILE"], ["860", "UZBEKISTAN"], ["068", "BOLIVIA"], ["566", "NIGERIA"],
  ["686", "SENEGAL"], ["478", "MAURITANIA"], ["024", "ANGOLA"], ["124", "CANADA"],
  ["418", "LAOS"], ["804", "UKRAINE"], ["004", "AFGHANISTAN"], ["268", "GEORGIA"],
  ["364", "IRAN"], ["818", "EGYPT"], ["188", "COSTA RICA"], ["400", "JORDAN"],
  ["120", "CAMEROON"], ["044", "BAHAMAS"], ["724", "SPAIN"], ["706", "SOMALIA"],
  ["288", "GHANA"], ["826", "UNITED KINGDOM"], ["524", "NEPAL"], ["586", "PAKISTAN"],
  ["608", "PHILIPPINES"], ["591", "PANAMA"], ["324", "GUINEA"], ["050", "BANGLADESH"],
  ["410", "SOUTH KOREA"], ["084", "BELIZE"], ["032", "ARGENTINA"], ["180", "DEM REP OF THE CONGO"],
  ["360", "INDONESIA"], ["404", "KENYA"], ["231", "ETHIOPIA"], ["583", "MICRONESIA, FEDERATED STATES OF"],
  ["104", "BURMA"], ["729", "SUDAN"], ["417", "KYRGYZSTAN"], ["584", "MARSHALL ISLANDS"],
  ["430", "LIBERIA"], ["051", "ARMENIA"], ["380", "ITALY"], ["328", "GUYANA"],
  ["616", "POLAND"], ["368", "IRAQ"], ["780", "TRINIDAD AND TOBAGO"], ["232", "ERITREA"],
  ["504", "MOROCCO"], ["762", "TAJIKISTAN"], ["398", "KAZAKHSTAN"], ["250", "FRANCE"],
  ["116", "CAMBODIA"], ["376", "ISRAEL"], ["764", "THAILAND"], ["760", "SYRIA"],
  ["620", "PORTUGAL"], ["178", "CONGO"], ["710", "SOUTH AFRICA"], ["112", "BELARUS"],
  ["031", "AZERBAIJAN"], ["372", "IRELAND"], ["858", "URUGUAY"], ["144", "SRI LANKA"],
  ["012", "ALGERIA"], ["008", "ALBANIA"], ["694", "SIERRA LEONE"], ["276", "GERMANY"],
  ["422", "LEBANON"], ["854", "BURKINA FASO"], ["070", "BOSNIA-HERZEGOVINA"], ["270", "GAMBIA"],
  ["716", "ZIMBABWE"], ["788", "TUNISIA"], ["064", "BHUTAN"], ["132", "CAPE VERDE"],
  ["466", "MALI"], ["498", "MOLDOVA"], ["384", "IVORY COAST"], ["768", "TOGO"],
  ["600", "PARAGUAY"], ["158", "TAIWAN"], ["887", "YEMEN"], ["348", "HUNGARY"],
  ["682", "SAUDI ARABIA"], ["800", "UGANDA"], ["646", "RWANDA"], ["562", "NIGER"],
  ["036", "AUSTRALIA"], ["226", "EQUATORIAL GUINEA"], ["834", "TANZANIA"], ["148", "CHAD"],
  ["528", "NETHERLANDS"], ["688", "SERBIA"], ["728", "SOUTH SUDAN"], ["056", "BELGIUM"],
  ["440", "LITHUANIA"], ["496", "MONGOLIA"], ["203", "CZECH REPUBLIC"], ["499", "MONTENEGRO"],
  ["383", "KOSOVO"], ["776", "TONGA"], ["428", "LATVIA"], ["100", "BULGARIA"],
  ["894", "ZAMBIA"], ["052", "BARBADOS"], ["204", "BENIN"], ["585", "PALAU"],
  ["554", "NEW ZEALAND"], ["300", "GREECE"], ["392", "JAPAN"], ["458", "MALAYSIA"],
  ["795", "TURKMENISTAN"], ["752", "SWEDEN"], ["308", "GRENADA"], ["882", "SAMOA"],
  ["242", "FIJI"], ["703", "SLOVAKIA"], ["624", "GUINEA-BISSAU"], ["659", "ST. KITTS-NEVIS"],
  ["434", "LIBYA"], ["414", "KUWAIT"], ["807", "NORTH MACEDONIA"], ["028", "ANTIGUA-BARBUDA"],
  ["480", "MAURITIUS"], ["670", "ST. VINCENT-GRENADINES"], ["266", "GABON"], ["191", "CROATIA"],
  ["262", "DJIBOUTI"], ["454", "MALAWI"], ["784", "UNITED ARAB EMIRATES"], ["756", "SWITZERLAND"],
  ["208", "DENMARK"], ["233", "ESTONIA"], ["140", "CENTRAL AFRICAN REPUBLIC"], ["508", "MOZAMBIQUE"],
  ["450", "MADAGASCAR"], ["246", "FINLAND"], ["598", "PAPUA NEW GUINEA"], ["705", "SLOVENIA"],
  ["072", "BOTSWANA"], ["516", "NAMIBIA"], ["040", "AUSTRIA"], ["702", "SINGAPORE"],
  ["048", "BAHRAIN"], ["578", "NORWAY"], ["408", "NORTH KOREA"], ["020", "ANDORRA"],
  ["196", "CYPRUS"], ["470", "MALTA"], ["352", "ICELAND"], ["096", "BRUNEI"],
  ["174", "COMOROS"], ["634", "QATAR"], ["492", "MONACO"], ["748", "ESWATINI"]
]);

const allTotalArrestsMap = new Map(
  Array.from(fullCountryMap, ([id, name]) => {
    const count = allCountryNames.find(d => d.country === name)?.count ?? null;
    return [id, count];
  })
);

const heroMap = Plot.plot({
  width: window.innerWidth,
  height: window.innerHeight,
  projection: { type: "equal-earth" },
  style: { background: "transparent" },
  color: { type: "log", scheme: "YlOrRd", unknown: "#2a2a2a" },
  marks: [
    Plot.geo(countries110, {
      fill: d => allTotalArrestsMap.get(d.id) ?? null,
      stroke: "#111",
      strokeWidth: 0.3,
    })
  ]
});
```

<div class="hero">
  <div class="hero-map">${heroMap}</div>
  <div class="hero-text">
    <div class="hero-kicker">An Analysis of ICE Arrest Data, September 2022 – March 2026</div>
    <div class="hero-title">Invisible Targets</div>
    <div class="hero-subtitle">What the data on 713,464 ICE arrests reveals about gender, race,<br>and who gets left out of the conversation.</div>
    <div class="hero-byline"><strong>Luis E. Banegas</strong> &nbsp;·&nbsp; May 23, 2026</div>
  </div>
</div>

<div class="article-body">
<div class="drop-cap">

<p> Between September 2022 and March 2026, ICE arrested 713,464 immigrants across the United States. Researchers have tracked their nationalities, where they were apprehended, and where they were deported.</p>

<p>But not their gender.</p>

<p>Although the vast majority of those arrested have been Latino and Caribbean men, no one is talking about it.</p>

<p>This project addresses that gap.</p>

<div class="article-body">
<div class="section-label">Who ICE Arrests</div>
<p>Of the 713,464 people arrested, 602,654 were men and 108,531 were women. The remaining 2,278 were recorded as unknown. Men outnumbered women by a ratio of nearly six to one. </p>
</div>

```js
display(Plot.plot({
  title: "Gender Distribution of ICE Arrests",
  marginLeft: 80,
  style: { background: "#f5f4f0", fontFamily: "Source Serif 4, Georgia, serif" },
  color: { range: ["#8b1a1a", "#2c3e50", "#95a5a6"] },
  marks: [
    Plot.barY(
      Array.from(genderCounts, ([gender, count]) => ({ gender, count })),
      { x: "gender", y: "count", fill: "gender", tip: true }
    ),
    Plot.ruleY([0])
  ]
}))
```

<p class="chart-caption">Source: Deportation Data Project. Hover over bars to see exact counts.</p>

<div class="article-body">
<div class="section-label">Where They Come From</div>
<p>Ten countries account for 88.3 percent of all arrests. Mexico leads with 255,344. Guatemala, Honduras, and Venezuela follow. Across every one of these countries, men vastly outnumber women. The pattern does not waver by nationality. It holds in every case, without exception.</p>
</div>

```js
const topCountries = Array.from(
  d3.rollup(cleanData, v => v.length, d => d.citizenship_country),
  ([country, count]) => ({ country, count })
).sort((a, b) => b.count - a.count).slice(0, 10);

const top10Countries = topCountries.map(d => d.country);
const genderByCountry = d3.rollup(
  cleanData.filter(d => top10Countries.includes(d.citizenship_country)),
  v => v.length,
  d => d.citizenship_country,
  d => d.gender
);

const genderByCountryFlat = Array.from(genderByCountry, ([country, genders]) =>
  Array.from(genders, ([gender, count]) => ({ country, gender, count }))
).flat();

display(Plot.plot({
  title: "Gender Breakdown Among Top 10 Countries of Citizenship",
  marginLeft: 120,
  style: { background: "#f5f4f0", fontFamily: "Source Serif 4, Georgia, serif" },
  color: { legend: true, range: ["#8b1a1a", "#2c3e50", "#95a5a6"] },
  marks: [
    Plot.barX(genderByCountryFlat, {
      y: "country",
      x: "count",
      fill: "gender",
      sort: { y: "-x" },
      tip: true
    }),
    Plot.ruleX([0])
  ]
}))
```

<p class="chart-caption">Source: Deportation Data Project. These ten countries accounted for 88.3% of all arrests. Hover over bars to see the breakdown by gender.</p>

<div class="article-body">
<div class="section-label">A Global Pattern</div>
<p>ICE arrested people from 206 countries. The concentration in Latin America and the Caribbean is unmistakable, but enforcement reached every continent. Hover over any country to see the total arrests and gender breakdown.</p>
</div>

```js
display(Plot.plot({
  title: "ICE Arrests by Country of Citizenship",
  width: 900,
  height: 500,
  style: { background: "#f5f4f0", fontFamily: "Source Serif 4, Georgia, serif" },
  projection: { type: "equal-earth" },
  color: { legend: true, type: "log", scheme: "YlOrRd", label: "Total Arrests", unknown: "#e0e0e0" },
  marks: [
    Plot.geo(countries110, {
      fill: d => allTotalArrestsMap.get(d.id) ?? null,
      stroke: "white",
      strokeWidth: 0.5,
      tip: true,
      title: d => {
        const name = fullCountryMap.get(d.id);
        if (!name) return null;
        const count = allTotalArrestsMap.get(d.id);
        if (!count) return null;
        const genders = allGenderByCountry.get(name);
        return `${name}\nTotal: ${count.toLocaleString()}\nMale: ${(genders?.get("Male") ?? 0).toLocaleString()}\nFemale: ${(genders?.get("Female") ?? 0).toLocaleString()}\nUnknown: ${(genders?.get("Unknown") ?? 0).toLocaleString()}`;
      }
    })
  ]
}))
```

<p class="chart-caption">Source: Deportation Data Project. Hover over any country to see total arrests and gender breakdown.</p>

<div class="article-body">
<div class="section-label">Where in the United States</div>
<p>Enforcement is concentrated in border states, but it reaches every corner of the country. Texas alone recorded over 153,000 arrests. Florida recorded nearly 54,000. New York, over 30,000. Georgia, nearly 19,000. In every state, men account for the overwhelming majority of those arrested. Hover over any state to see the numbers.</p>
</div>

```js
const usMap = await fetch("https://cdn.jsdelivr.net/npm/us-atlas@3/states-10m.json").then(r => r.json());
const usStates = feature(usMap, usMap.objects.states);

const stateArrestData = d3.rollup(cleanData, v => v.length, d => d.apprehension_state);
const stateGenderData = d3.rollup(cleanData, v => v.length, d => d.apprehension_state, d => d.gender);

const stateNameToId = new Map([
  ["ALABAMA", "01"], ["ALASKA", "02"], ["ARIZONA", "04"], ["ARKANSAS", "05"],
  ["CALIFORNIA", "06"], ["COLORADO", "08"], ["CONNECTICUT", "09"], ["DELAWARE", "10"],
  ["DISTRICT OF COLUMBIA", "11"], ["FLORIDA", "12"], ["GEORGIA", "13"], ["HAWAII", "15"],
  ["IDAHO", "16"], ["ILLINOIS", "17"], ["INDIANA", "18"], ["IOWA", "19"],
  ["KANSAS", "20"], ["KENTUCKY", "21"], ["LOUISIANA", "22"], ["MAINE", "23"],
  ["MARYLAND", "24"], ["MASSACHUSETTS", "25"], ["MICHIGAN", "26"], ["MINNESOTA", "27"],
  ["MISSISSIPPI", "28"], ["MISSOURI", "29"], ["MONTANA", "30"], ["NEBRASKA", "31"],
  ["NEVADA", "32"], ["NEW HAMPSHIRE", "33"], ["NEW JERSEY", "34"], ["NEW MEXICO", "35"],
  ["NEW YORK", "36"], ["NORTH CAROLINA", "37"], ["NORTH DAKOTA", "38"], ["OHIO", "39"],
  ["OKLAHOMA", "40"], ["OREGON", "41"], ["PENNSYLVANIA", "42"], ["RHODE ISLAND", "44"],
  ["SOUTH CAROLINA", "45"], ["SOUTH DAKOTA", "46"], ["TENNESSEE", "47"], ["TEXAS", "48"],
  ["UTAH", "49"], ["VERMONT", "50"], ["VIRGINIA", "51"], ["WASHINGTON", "53"],
  ["WEST VIRGINIA", "54"], ["WISCONSIN", "55"], ["WYOMING", "56"], ["PUERTO RICO", "72"]
]);

const idToStateName = new Map(Array.from(stateNameToId, ([name, id]) => [id, name]));
const stateArrestById = new Map(
  Array.from(stateNameToId, ([name, id]) => [id, stateArrestData.get(name) ?? null])
);

display(Plot.plot({
  title: "ICE Arrests by State",
  width: 900,
  height: 560,
  style: { background: "#f5f4f0", fontFamily: "Source Serif 4, Georgia, serif" },
  projection: "albers-usa",
  color: { legend: true, type: "log", scheme: "YlOrRd", label: "Total Arrests", unknown: "#e0e0e0" },
  marks: [
    Plot.geo(usStates, {
      fill: d => stateArrestById.get(d.id) ?? null,
      stroke: "white",
      strokeWidth: 0.5,
      tip: true,
      title: d => {
        const name = idToStateName.get(d.id);
        if (!name) return null;
        const total = stateArrestData.get(name);
        if (!total) return null;
        const genders = stateGenderData.get(name);
        return `${name}\nTotal: ${total.toLocaleString()}\nMale: ${(genders?.get("Male") ?? 0).toLocaleString()}\nFemale: ${(genders?.get("Female") ?? 0).toLocaleString()}\nUnknown: ${(genders?.get("Unknown") ?? 0).toLocaleString()}`;
      }
    })
  ]
}))
```

<p class="chart-caption">Source: Deportation Data Project. Hover over each state to see total arrests and gender breakdown.</p>

<div class="article-body">
<div class="section-label">The Surge</div>
<p>Then came January 2025.</p>
<p>Deportations that had held steady at around 5,000 per month surged past 28,000. Men bore that surge almost entirely. In 2023, men accounted for 57 percent of removals. In 2024, 75.8 percent. In 2025, the first full calendar year of the second Trump Administration, more men were arrested and removed than in the entire Biden era combined.</p>
<p>Of the 349,467 people who received a final order of removal, 318,285 were men. 31,182 were women. That is not a rounding error. That is a pattern sustained across every year, every nationality, and every criminal category in the dataset.</p>
</div>

```js
const deported = cleanData.filter(d => d.departed_date !== null);

const deportationsByMonthGender = d3.rollup(
  deported.filter(d => {
    const date = new Date(d.departed_date);
    return date >= new Date("2022-09-01") && date < new Date("2026-03-01");
  }),
  v => v.length,
  d => d3.timeMonth.floor(new Date(d.departed_date)),
  d => d.gender
);

const deportationsFlat2 = Array.from(deportationsByMonthGender, ([date, genders]) =>
  Array.from(genders, ([gender, count]) => ({ date, gender, count }))
).flat().sort((a, b) => a.date - b.date);

display(Plot.plot({
  title: "ICE Deportations Over Time by Gender",
  width: 900,
  height: 400,
  marginLeft: 80,
  marginRight: 40,
  style: { background: "#f5f4f0", fontFamily: "Source Serif 4, Georgia, serif" },
  x: { label: "Month", type: "utc", domain: [new Date("2022-09-01"), new Date("2026-03-01")] },
  y: { label: "Deportations", grid: true },
  color: { legend: true, range: ["#8b1a1a", "#2c3e50", "#95a5a6"] },
  marks: [
    Plot.lineY(deportationsFlat2, {
      x: "date",
      y: "count",
      stroke: "gender",
      strokeWidth: 2,
      tip: true,
      title: d => `${d.gender}: ${d.count.toLocaleString()}`
    }),
    Plot.dot(deportationsFlat2.filter(d => d.date.getTime() === new Date("2022-09-01").getTime()), {
      x: "date",
      y: "count",
      stroke: "gender",
      fill: "gender",
      r: 4
    }),
    Plot.ruleY([0])
  ]
}))
```

<p class="chart-caption">Source: Deportation Data Project. The sharp spike beginning January 2025 corresponds to the start of the second Trump Administration. Hover over lines to see monthly counts by gender.</p>

<div class="closing">
<p>The gender dimension of immigration enforcement is not a side story. It is the story the data keeps telling, year after year, state after state, country after country. Latino and Caribbean men are the primary target of the deportation regime. The numbers are unambiguous. The question is whether we are willing to see them.</p>
</div>

<p class="footnote">Data: Deportation Data Project, September 2022 – March 2026.</p>