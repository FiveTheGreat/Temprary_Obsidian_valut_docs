#### Padding
---
###### ==Naming:==

| name    | mean                  |
| ------- | --------------------- |
| p       | padding               |
| x       | x-axis                |
| y       | y-axis                |
| l       | left                  |
| r       | right                 |
| b       | bottom                |
| t       | top                   |
| s       | Inline-start          |
| e       | Inline-end            |
| m       | margin                |
| space   | space-between         |
| reverse | space-between-reverse |
| mx      | max                   |

> Example:

`p-0`  padding : 0
`px-0`  padding left && right : 0
`py-0`  padding top && bottom : 0
`ps-0`  padding Inline start : 0
`pe-0`  padding Inline end : 0
`pt-0`  padding top :0
`pb-0`  padding bottom :0
`pl-0`  padding left:0
`pr-0`  padding right:0
`m-0`    margin : 0
`mx-0`  margin left && right : 0
`my-0`  margin top && bottom : 0
`space-x-0`  margin-left:0
`space-y-0`  margin-top:0
`space-x-reverse`  space-between-reverse

> Note:

if you add space between reverse 

```html
<div class="flex flex-row-reverse space-x-4 space-x-reverse ...">
  <div>01</div>
  <div>02</div>
  <div>03</div>
</div>
```

---
###### ==Sizing:==

| name | mean | rem   |
| ---- | ---- | ----- |
| px   | 1px  |       |
| 0.5  | 2px  | 0.125 |
| 1    | 4px  | 0.25  |
| 1.5  | 6px  | 0.375 |
| 2    | 8px  | 0.50  |
>Example:

`px-px`  padding left && right : 1px
`px-0.5`  padding left && right : 2px 
`etc...`

---


