# fullpage-vue

> A sigle-page scroll plugin based on vue@2.x,support for mobile and PC .
> [中文版](./README_zh.md)

## Changelog by DelanoRochan

- Enable / disable touch events toggle, i.e. it enables scroll inside page with `overflow-y:scroll`
- Enable / disable scroll toggle
- Remove console.log
-

## Usage by DelanoRochan

```
opts: {
        start: -1,
        dir: "v",
        der: 0.1,
        loop: false,
        duration: 100,
        touchEnabled: false,
        scrollEnabled: false,
        beforeChange: (el, prev, next) => {
          this.index = next;
        },
        afterChange: (el, prev, next) => {
          this.index = prev;
          if (this.index != 1) {
            // Update the URL
            this.$router.push("/#" + this.getKey(this.index));
          } else {
            // At start, there's no hash, after click or moveTo action, we update the URL
            if (this.$router.currentRoute.hash.split("#")[1] != undefined) {
              this.$router.push("/#" + this.getKey(this.index));
            }
          }
        },
        animateClass: "animCustom"
      }
```

## Overview

To achieve sigle-page scroll in mobile, support horizontal scroll and vertical scroll, support all the animation instructions of animate.css.

## Online demo

here's a [jsfiddle demo](https://jsfiddle.net/6jc3okaq/98/)
[Live demo](https://river-lee.github.io/vue-fullpage/examples)

## Installation

````

npm install fullpage-vue --save

```

If you want use animate instruction, please install animate.css

```

npm install animate.css --save

````

[animate.css usage](https://daneden.github.io/animate.css/)

## Document

### options

- `start` : (default:`0`) Display first page
- `duration` : (default:`500`)
- `loop` : (default:`false`)
- `dir` : (default:`v`) Direction of movement
- `der` : (default:`0.1`)
- `movingFlag` : (default:`false`)
- `beforeChange` : (default:`function`) Before change callback
- `afterChange` : (default:`function`) After change callback
- `overflow` : (default:`hidden`) hidden || scroll || auto
  `hidden` Hidden overflow
  `scroll` Handling the scroll bars of page
  `auto` Handling all scroll bars in page,Start checking from triggered elements
- `disabled` : (default:`false`)

### method

#### moveTo

Move to the specified page

#### movePrev

Move to the previous page

#### moveNext

Move to the next page

#### setDisabled

Change the value of disabled. A value of true disables move

#### \$update

Update the dom structure,for example `v-for` and `v-if` Affect the number of pages, need to manually call `$update`

```html
<button
  type="button"
  v-for="btn in pageNum"
  :class="{active:index == btn + 2}"
  @click="moveTo(btn+2)"
>
  page {{btn+2}}
</button>
<button type="button" @click="showPage()">add page</button>

<div class="page-2 page" v-for="page in pageNum">
  <h2 class="part-2" v-animate="{value: 'bounceInRight'}">page {{page}}</h2>
</div>
```

```js
    showPage:function(){
      this.pageNum ++;
      this.$refs.fullpage.$fullpage.$update();
    }
```

## getting started

#### main.js

Import the plugin of css and js file in main.js

```js
import "animate.css";
import "fullpage-vue/src/fullpage.css";
import VueFullpage from "fullpage-vue";
Vue.use(VueFullpage);
```

#### app.vue

**template**

`fullpage-container`、`fullpage-wp`、`page`are default class name.
Add the `v-fullpage` command to the `page-wp` container.
Add the `v-animate` command to the `page` container.

```html
<div class="fullpage-container">
  <div class="fullpage-wp" v-fullpage="opts" ref="example">
    <div class="page-1 page">
      <p class="part-1" v-animate="{value: 'bounceInLeft'}">fullpage-vue</p>
    </div>
    <div class="page-2 page">
      <p class="part-2" v-animate="{value: 'bounceInRight'}">fullpage-vue</p>
    </div>
    <div class="page-3 page">
      <p class="part-3" v-animate="{value: 'bounceInLeft', delay: 0}">
        fullpage-vue
      </p>
      <p class="part-3" v-animate="{value: 'bounceInRight', delay: 600}">
        fullpage-vue
      </p>
      <p class="part-3" v-animate="{value: 'zoomInDown', delay: 1200}">
        fullpage-vue
      </p>
    </div>
  </div>
  <button @click="moveNext">next</button>
</div>
```

**script**

`fullpage-vue` value please refer to [api document](https://github.com/river-lee/vue-fullpage#options)

```js
export default {
  data() {
    return {
      opts: {
        start: 0,
        dir: "v",
        duration: 500,
        beforeChange: function(currentSlideEl, currenIndex, nextIndex) {},
        afterChange: function(currentSlideEl, currenIndex) {}
      }
    };
  },
  method: {
    moveNext() {
      this.$refs.example.$fullpage.moveNext(); //Move to the next page
    }
  }
};
```

**style**

Set the `page-container` container's width and height what do you want, and the `v-fullpage` command will adapt the width and height of the parent element.
The following settings allow the scrolling page to fill the full screen.

```
<style>
.fullpage-container {
  position: absolute;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
}
</style>
```
