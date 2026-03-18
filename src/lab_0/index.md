# Gender, Enforcement, and Power in US Immigration Policy 
## Trends in ICE Apprehensions Between 2023 and 2025


##### Welcome to my first lab! I'm Luis, a PhD student in sociology at the CUNY Graduate Center. This project explores how gender shapes outcomes in the US immigration enforcement pipeline. Using recent ICE apprehension data, I analyze who is arrested, how cases move through the system, and whether enforcement patterns differ between men and women. Below, you'll find an interactive table and visualizations from my statistical analysis. 



<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>2023-2025 Top Ten Countries of Arrests</title>
  <style>
    body { font-family: Aptos, sans-serif; padding: 20px; }
    h2 { margin: 0 0 12px; }
    table { border-collapse: collapse; width: 760px; max-width: 100%; }
    th, td { border: 1px solid #2b2b2b; padding: 10px 12px; }
    thead th { background:rgb(111, 176, 214); color: #000; text-align: left; }
    tbody td { background: #cfe6f3; }
    td.num, th.num { text-align: right; font-variant-numeric: tabular-nums; }
    tbody tr.total td { font-weight: 700; }
    caption { caption-side: top; text-align: left; font-weight: 700; margin-bottom: 8px; }
  </style>
</head>
<body>

  <table>
    <caption>2023-2025 Top Ten Countries of Arrests</caption>
    <thead>
      <tr>
        <th>Country</th>
        <th class="num">Female</th>
        <th class="num">Male</th>
        <th class="num">Total Number of arrests</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>Mexico</td>
        <td class="num">10,031</td>
        <td class="num">130,653</td>
        <td class="num">140,684</td>
      </tr>
      <tr>
        <td>Guatemala</td>
        <td class="num">4,100</td>
        <td class="num">43,369</td>
        <td class="num">47,469</td>
      </tr>
      <tr>
        <td>Honduras</td>
        <td class="num">4,520</td>
        <td class="num">35,909</td>
        <td class="num">40,429</td>
      </tr>
      <tr>
        <td>Venezuela</td>
        <td class="num">3,665</td>
        <td class="num">17,713</td>
        <td class="num">21,378</td>
      </tr>
      <tr>
        <td>Nicaragua</td>
        <td class="num">4,273</td>
        <td class="num">13,469</td>
        <td class="num">17,742</td>
      </tr>
      <tr>
        <td>El Salvador</td>
        <td class="num">2,148</td>
        <td class="num">14,689</td>
        <td class="num">16,837</td>
      </tr>
      <tr>
        <td>Colombia</td>
        <td class="num">3,370</td>
        <td class="num">9,528</td>
        <td class="num">12,898</td>
      </tr>
      <tr>
        <td>Ecuador</td>
        <td class="num">3,168</td>
        <td class="num">8,969</td>
        <td class="num">12,137</td>
      </tr>
      <tr>
        <td>Cuba</td>
        <td class="num">1,658</td>
        <td class="num">7,048</td>
        <td class="num">8,706</td>
      </tr>
      <tr>
        <td>Dominican Republic</td>
        <td class="num">794</td>
        <td class="num">5,468</td>
        <td class="num">6,262</td>
      </tr>
      <tr class="total">
        <td>Total</td>
        <td class="num">37,727</td>
        <td class="num">286,815</td>
        <td class="num">324,542</td>
      </tr>
    </tbody>
  </table>
<div class="source">
  <p><strong>Source:</strong> Data gathered from the Deportation Data Project.</p>
  <p><strong>Code Source:</strong> 
    <a href="https://www.htmlgoodies.com/html5/advanced-html-table-features/" target="_blank">
      https://www.htmlgoodies.com/html5/advanced-html-table-features/
    </a>
  </p>
</div>
</body>
</html>

# Conclusions
<ul>
  <li>Mexican nationals have the highest total arrests (2023–2025).</li>
  <li>Men account for most arrests across all top countries.</li>
  <li>Women’s arrests are smaller but still substantial in several countries.</li>
</ul>


# Instagram Picture Attached
<div style="margin-top:20px; text-align:center;">
  <blockquote class="instagram-media"
    data-instgrm-permalink="https://www.instagram.com/p/DSnl0UtEzlx/"
    data-instgrm-version="14">
  </blockquote>
</div>

<script async src="https://www.instagram.com/embed.js"></script>
<div class="source">
  <p><strong>Source:</strong> The Guardian.</p>
  <p><strong>Code Source:</strong> 
    <a href="https://www.commoninja.com/blog/providing-sample-html-code-for-manually-embedding-instagram-feeds#Sample-HTML-Code-for-an-Instagram-Feed-Widget" target="_blank">
      https://www.commoninja.com/blog/providing-sample-html-code-for-manually-embedding-instagram-feeds#Sample-HTML-Code-for-an-Instagram-Feed-Widget
    </a>
  </p>
</div>
</body>
</html>

# Fun Fact: Did you know that in the wild, orangutan babies learn necessary survival skills by observing their mothers? They have one of the longest interbirth intervals of any mammal. 
## This is Jambi the Orangutan. I met him at the Philly Zoo last year. 

<img src="./IMG_8351.jpg" width="400">

## Interactive Element

```js
import * as Inputs from "npm:@observablehq/inputs";

// Create a dropdown input using view()
const topic = view(
  Inputs.select(
    ["ICE Arrests", "Court Outcomes", "Detainers"],
    {
      label: "Choose topic:",
      value: "ICE Arrests"
    }
  )
);

// Display a visible reference that changes
html`<h3>You selected: <strong>${topic}</strong></h3>`