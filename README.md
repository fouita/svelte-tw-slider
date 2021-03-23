# Svelte and Tailwind Slider component

Form range (slider) component built with svelte and tailwindcss

# Installation
```bash
    $npm i -D @fouita/slider
```

# Usage

## Single value range

![slider fouita](https://cdn.fouita.com/assets/pics/template/slider/slider-simple.jpg)

> image slider-simple


```html
<script>
	import Slider from '@fouita/slider'					
	let value = 21
</script>

<div class="p-6 text-3xl text-center">
	{value}	
</div>
<Slider class="mt-5 mx-6" min={0} max={30} bind:value />
```

## Multi value range

Multi values works with array of two values, like the following

![slider fouita](https://cdn.fouita.com/assets/pics/template/slider/slider-range.jpg)

```html
<script>
	import Slider from '@fouita/slider'					
	let value = [10,21]
</script>

<div class="p-6 text-3xl text-center">
	{value}	
</div>
<Slider class="mt-5 mx-6" min={0} max={30} bind:value />
```

## With tooltip

By adding `tooltip` prop we can show the value when sliding

![slider fouita](https://cdn.fouita.com/assets/pics/template/slider/slider-tooltip.jpg)


```html
<Slider tooltip min={0} max={30} bind:value />
```

We can show the tooltip when hovering over the slider pointer by adding `tooltip=hover`

```html
<Slider tooltip=hover min={0} max={30} bind:value />
```

## Custom color

We can change the color by adding `color` prop

![slider fouita](https://cdn.fouita.com/assets/pics/template/slider/slider-pink.jpg)

```html
<Slider tooltip=hover color=pink-600 min={0} max={30} bind:value />
```

## Custom labels

To change the labels min/max we need to attribute `minLabel` & `maxLabel`. We can also remove them by using an empty string.

![slider fouita](https://cdn.fouita.com/assets/pics/template/slider/slider-mlabel.jpg)

```html
<Slider tooltip=hover minLabel='$0' maxLabel='Expensive' min={0} max={30} bind:value />
```

## Custom indicators 

To change the values on the tooltip we can add a value format by using `valueLabel` prop.
In case you need to use only one value (not a range) you can add a simple string `valueLabel="$%d"`

![slider fouita](https://cdn.fouita.com/assets/pics/template/slider/slider-vlabel.jpg)

```html
<Slider valueLabel={['%d €','%d €']} tooltip min={0} max={30} bind:value />
```

## Custom scale

We can use custom array of values and show the desired label when we hit a specific number.
Assuming a scale of skills `[basic, medium, advanced, expert]`

* 0-5 : basic 
* 5-15 : medium
* 15-25: advanced
* 25-30: expert

![slider fouita](https://cdn.fouita.com/assets/pics/template/slider/slider-scale.jpg)

```html
<script>
  import Slider from '@fouita/slider'

  let skill_level=['Basic','Medium','Advanced','Expert']
  let vlevel=10 // medium by default 

</script>

<Slider bind:value={vlevel} min={0} max={30} minLabel="Basic" maxLabel="Expert" valueLabel={skill_level[Math.round(vlevel/10)]} tooltip="hover" />

```



## About

[Fouita : UI framework for svelte + tailwind components](https://fouita.com)


