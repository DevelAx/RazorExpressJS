# RazJS
## JavaScript (browser) version of the [Razor-Express (RAZ) view template engine library](https://www.npmjs.com/package/raz).

> <pre>$ npm install <b>razjs</b> --save</pre>

>*Note:* debugging information of runtime code errors in the JS version is more stingy because the NodeJS virtual machine is not used in the browser. 

### Examples

#### Example 1: rendering HTML with Razor syntax

*HTML:*
```HTML
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
<sup>^ Try it on [jsfiddle.net](https://jsfiddle.net/develax/tfr9zhm5/)</sup>

#### Example 2: handling and displaying errors

*HTML:*
```HTML
<div id="target">
</div>
```

*JavaScript:*
```JS
window.addEventListener('error', function(e) {            
    if (!e.error.isRazorError) return;
    e.preventDefault(); // stop propagating

    setTimeout(()=>{
        document.documentElement.innerHTML = e.error.html();
    }, 0);
});

const num = 1;
const template = `
<div>
 @Model
</span>`;

const html = raz.render(template, num);
document.getElementById("target").innerHTML = html;

```


----------------
> More syntax construction examples on [Razor-Express syntax reference for NodeJS & Express](https://github.com/DevelAx/RazorExpress/blob/master/docs/syntax.md).
