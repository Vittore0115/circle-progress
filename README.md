# v-circle-progress

![](https://img.shields.io/badge/webpack-3.9.1-blue.svg)
![](https://img.shields.io/badge/vue-2.6.14-brightgreen.svg)
![](https://img.shields.io/badge/Author-逸成-yellow.svg)

## npm address

npm address in here : https://www.npmjs.com/package/v-circle-progress

## Explain

1 . Circle progress

2 . Support customization, such as progress bar color, ring size, position, text...

3 . .....(There will be more follow ups)

---

## Usage

### 1.1 Installation

```javascript
  npm i v-circle-progress
```

### 1.2 ES6 Import

```javascript
import vCircleProgress from 'v-circle-progress'

export default {
	components: {
		vCircleProgress,
	},
}
```

## Basic Example

html

```html
<template>
	<div>
		<vCircleprogress
			:title="title"
			:content="content"
			:viewDetails="viewDetails"
			:rate="currentRate"
			:lineColor="lineColor"
			:backColor="backColor"
			:width="width"
			@jumpPage="jumpPageHandle"
		/>
	</div>
</template>
```

js

```javascript
import vCircleProgress from 'v-circle-progress'

export default {
	components: {
		vCircleProgress,
	},
	data() {
		return {
			title: '剩余流量',
			content: '100GB',
			viewDetails: '查看详情',
			currentRate: 0.5,
			width: 150,
			lineColor: '#42b983',
			backColor: 'grey',
			cssStyle: {
				titleColor: 'black',
				textColor: '#42b983',
				viewDetailsColor: '#42b983',
			},
		}
	},
	methods: {
		// handle
		jumpPageHandle(val) {
			console.log('val:',val)
		}
	},
}
```

### Props

|    props    |  type  |      default       |                                    description                                     |
| :---------: | :----: | :----------------: | :--------------------------------------------------------------------------------: |
|    title     | String |       剩余流量       |                               Progress bar Title                                 |
|   content   | String |         100GB |                                     Progress bar Content                                 |
| viewDetails  | String |        查看详情        |                                Button Text                                      |
| currentRate  | Number |         0.5        |                                Progress current rate                               |
| width  | Number |               150        |                                          Circle size                                      |
| lineColor  | String |         #42b983        |                                 Progress bar color                                    |
| backColor  | String |          grey        |                                 Progress bar backColor                                |
|  cssStyle   | Object |          {}         |                                     Circle Style                                     |
---

`Here is the props of **cssStyle**`

|      props      |  type  | default |          description           |
| :-------------: | :----: | :-----: | :----------------------------: |
|  titleColor   | String |  black |            title text color       |
|  textColor  | String | #42b983 |               text color           |
|  viewDetailsColor  | String | #42b983 | Button background color     |

### Event

When you press the circle  **viewDetails** button，trigger `jumpPageHandle()`

```javascript
    jumpPageHandle(rate) {
      console.log('current rate is :', rate)
    }
```

