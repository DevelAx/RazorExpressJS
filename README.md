# RazJS
## JavaScript (browser) version of the [Razor-Express (RAZ) view template engine library](https://www.npmjs.com/package/raz).

> <pre>$ npm install <b>razjs</b> --save</pre>

>*Note:* debugging information of runtime code errors in the JS version is more stingy because the NodeJS virtual machine is not used in the browser. 

### Examples

#### Example 1: rendering HTML with Razor syntax

*HTML:*
```HTML
<script src="node_modules/razjs/raz.js"></script>
<div id="target">
</div>
```

*JavaScript:*
```JS
const countries = [
 { name: "Russia", area: 17098242 },
 { name: "Canada", area: 9984670 },
 { name: "United States", area: 9826675 },
 { name: "China", area: 9596960 },
 { name: "Brazil", area: 8514877 },
 { name: "Australia", area: 7741220 },
 { name: "India", area: 3287263 }
];

const template = `
<table>
  <tr>
    <th>Country</th>
    <th>Area sq.km</th>
  </tr>
  @for(var i = 0; i < Model.length; i++){
    var country = Model[i];
    <tr>
      <td>@country.name</td>
      <td>@country.area</td>
    </tr>
  }
</table>`;

const html = raz.render(template, countries);
document.getElementById("target").innerHTML = html;

```
*HTML output:*
<pre>
<table>
  <tbody><tr>
    <th>Country</th>
    <th>Area sq.km</th>
  </tr>
    <tr>
      <td>Russia</td>
      <td>17098242</td>
    </tr>
    <tr>
      <td>Canada</td>
      <td>9984670</td>
    </tr>
    <tr>
      <td>United States</td>
      <td>9826675</td>
    </tr>
    <tr>
      <td>China</td>
      <td>9596960</td>
    </tr>
    <tr>
      <td>Brazil</td>
      <td>8514877</td>
    </tr>
    <tr>
      <td>Australia</td>
      <td>7741220</td>
    </tr>
    <tr>
      <td>India</td>
      <td>3287263</td>
    </tr>
</tbody></table>
</pre>
<sup>^ Try it on [jsfiddle.net](https://jsfiddle.net/develax/tfr9zhm5/)</sup>

#### Example 2: handling and displaying errors

*HTML:*
```HTML
<script src="node_modules/razjs/raz.js"></script>
<div id="target">
</div>
```

*JavaScript:*
```JS
window.addEventListener('error', function (e) {
     if (!e.error.isRazorError) return;
     setTimeout(() => {
         // The document have to be fully loaded before replacing its whole content - that's why we use timer.
         document.documentElement.innerHTML = e.error.html();
     }, 0);
     e.preventDefault(); // Stop propagating since we've handled it.
 });

const num = 1;
const template = `
<div>
 @Model
</span>`;

const html = raz.render(template, num);
document.getElementById("target").innerHTML = html;

```

#### Example 3 (the entire HTML-page with JavaScript embedded)

```HTML
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>RazJS library test - #1</title>
    <style>
        pre { 
            background-color:black; 
            color:#f3eeb5; 
            padding: 0 0.5rem; 
            border: 1rem ridge #ababab;
        }
    </style>
    <script src="node_modules/razjs/raz.js"></script>
    <script type="text/javascript">
        function test() {
            var template = `
<h1>@Model.title</h1>
<strong>Days of the week:</strong>
<ul>
    @for (var i = 0; i < Model.days.length; i++) {
        <li>@Model.days[i]</li>
    }
</ul>
<div>Today is <i>@Model.today()</i>.</div>
<br />
<hr />
<div>
    <h2>The Razor-syntax template used in this example</h2>
    <pre>
        @Model.template
    </pre>
</div>`;
            var model = {
                title: document.title,
                day: new Date().getDay(),
                days: ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"],
                today: function () {
                    return this.days[this.day];
                },
                template
            }
            document.body.insertAdjacentHTML('afterbegin', raz.render(template, model));
        }
    </script>
</head>

<body onload="test()">
</body>

</html>
```
*HTML output:*

**Days of the week:**
 - Sunday
- Monday
- Tuesday
- Wednesday
- Thursday
- Friday
- Saturday
Today is _Thursday_.

<sup>^ Try it on [jsfiddle.net](https://jsfiddle.net/develax/ub5os9hn/4/)</sup>

----------------
> More syntax construction examples on [Razor-Express syntax reference for NodeJS & Express](https://github.com/DevelAx/RazorExpress/blob/master/docs/syntax.md).
