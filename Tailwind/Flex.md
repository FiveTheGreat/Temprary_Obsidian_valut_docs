==**Keyword**==
- flex
- basis
- row `row`
- col `column`
- auto 
- wrap
- nowrap `no-wrap`
- reverse
- container

`flex`  display:flex;
`basis-0`  flex-basis : 0px
`basis-1`  flex-basis : 4px
`basis-2`  flex-basis : 8px
`basis-auto`  flex-basis:auto
`basis-1/2`  flex-basis : 50%
`basis-1/4`   flex-basis : 25%
`basis-3/4`   flex-basis : 75%
`basis-1/5`   flex-basis : 20%
`basis-2/5`   flex-basis : 40%
`flex-row`  flex-direction : row
`flex-col`  flex-direction : column
`flex-row-reverse`  flex-direaction : row- reversed


> Customize-Spacing:

```js
module.exports = {
  theme: {
    extend: {
      spacing: {
        '112': '28rem',
        '128': '32rem',
      }
    }
  }
}
```
---
* `flex-1`   flex : 1 1 0%;

>Use `flex-1` to allow a flex item to grow and shrink as needed, ignoring its initial size:
---
* `flex-initial`  flex : 0 1 auto;

> Use `flex-initial` to allow a flex item to shrink but not grow, taking into account its initial size
---
* `flex-auto`  flex : 1 1 auto;

>Use `flex-auto` to allow a flex item to grow and shrink, taking into account its initial size:
---
* `flex-none`  flex : none;

>Use `flex-none` to prevent a flex item from growing or shrinking:
---

