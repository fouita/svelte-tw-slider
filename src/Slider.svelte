<script>
  import Tooltip from "@fouita/tooltip";
  import { onMount } from "svelte/internal";
  import { createEventDispatcher } from "svelte";
  let klass = "";
  
  const dispatch = createEventDispatcher();
  export { klass as class };
  export let min = 0;
  export let max = 100;
  export let value = [25, 64];

  export let minLabel = String(min);
  export let maxLabel = String(max);
  export let valueLabel = null;

  export let color = "indigo-600";
  export let tooltip = false;

  let indLabel = valueLabel;

  let current_pointer = null;

  function setValue(val) {
    if (Array.isArray(value)) {
      value[current_pointer == pointer ? 0 : 1] = val;
    } else {
      value = val;
    }
   dispatch('change', value)
  }

  function movePointer(e) {
    if (!current_pointer) return false;
    let cx = e.clientX || e.touches[0].clientX;
    let diff = cx - elm_x; // calculate percentage
    let per = (diff * 100) / cont_width;
    per = per < 0 ? 0 : per > 100 ? 100 : per;
    setValue(parseInt((per * (max - min)) / 100) + min);
  }

  let elm_x = null;
  let pointer = null;
  let pointer2 = null;
  let cont_width = null;
  let progress_bar = null;

  let display_tooltip1 = false;
  let display_tooltip2 = false;

  function elmPosition(node) {
    elm_x = node.offsetLeft;
  }

  function setPointer(node) {
    pointer = node;
  }

  function setPointer2(node) {
    pointer2 = node;
  }

  let container;
  function setContainer(node) {
    container = node;
    cont_width = node.clientWidth;
  }

  function progressBar(node) {
    progress_bar = node;
  }

  function mouseUp() {
    current_pointer = null;
    display_tooltip1 = tooltip == "hover" ? false : tooltip;
    display_tooltip2 = display_tooltip1;
  }

  $: if (progress_bar && pointer) {
    if (Array.isArray(value)) {
      value[0] = value[0] > min ? value[0] : min;
      value[1] = value[1] < max ? value[1] : max;

      let per1 = ((value[0] - min) * 100) / (max - min);
      pointer.style.left = `${per1}%`;
      let per2 = ((value[1] - min) * 100) / (max - min);
      pointer2.style.left = `${per2}%`;

      progress_bar.style.width = `${per2 - per1}%`;
      progress_bar.style.left = `${per1}%`;
    } else {
      value = value > min ? value : min;
      value = value < max ? value : max;
      let per = ((value - min) * 100) / (max - min);
      pointer.style.left = `${per}%`;
      progress_bar.style.width = `${per}%`;
    }
  }

  onMount(() => {
    minLabel = minLabel.replace("%d", min);
    maxLabel = maxLabel.replace("%d", max);
  });

  $: if (valueLabel !== null) {
    if (Array.isArray(valueLabel)) {
      if (valueLabel.find(vl => !~String(vl).indexOf("%d"))) {
        indLabel = valueLabel.map(v => String(v));
      } else {
        indLabel = valueLabel.map((lab, i) => lab.replace("%d", value[i]));
      }
    } else {
      if (!~valueLabel.indexOf("%d")) {
        indLabel = String(valueLabel);
      } else {
        indLabel = valueLabel.replace("%d", value);
      }
    }
  }

  $: if (valueLabel === null) {
    if (Array.isArray(value)) {
      indLabel = value.map(v => String(v));
    } else {
      indLabel = String(value);
    }
  }

  $: if (tooltip == true) {
    display_tooltip1 = true;
    display_tooltip2 = true;
  }

  function resizeWindow() {
    cont_width = container.clientWidth;
  }
</script>

<svelte:window
  on:mousemove={movePointer}
  on:touchmove={movePointer}
  on:mouseup={mouseUp}
  on:touchend={mouseUp}
  on:resize={resizeWindow} />
<div key="k1" class={klass}>
  <div key="k2" class="py-1 relative min-w-full" use:elmPosition>
    <div key="k3" class="h-1 -mt-px bg-gray-300 rounded-full" use:setContainer>
      <div
        key="k4"
        class="absolute h-1 rounded-full bg-{color} w-0"
        use:progressBar />
      <div
        key="k5"
        class="absolute h-4 -mt-1 flex items-center justify-center w-4
        rounded-full bg-white shadow border border-gray-300 -ml-2 top-0
        cursor-pointer"
        unselectable="on"
        onselectstart="return false;"
        use:setPointer
        on:touchstart={() => (current_pointer = pointer)}
        on:mousedown={() => (current_pointer = pointer)}
        on:mouseenter={() => (display_tooltip1 = tooltip || tooltip == 'hover')}
        on:mouseleave={() => (display_tooltip1 = (!!tooltip && !!current_pointer) || (tooltip == 'hover' ? false : tooltip))}>
        <div key="k6" class="relative -mt-2 w-1">
          <Tooltip
            text={(Array.isArray(indLabel) && indLabel[0]) || indLabel}
            show={display_tooltip1} />
        </div>
      </div>
      {#if Array.isArray(value)}
        <div
          key="k7"
          class="absolute h-4 -mt-1 flex items-center justify-center w-4
          rounded-full bg-white shadow border border-gray-300 -ml-2 top-0
          cursor-pointer"
          unselectable="on"
          onselectstart="return false;"
          use:setPointer2
          on:touchstart={() => (current_pointer = pointer2)}
          on:mousedown={() => (current_pointer = pointer2)}
          on:mouseenter={() => (display_tooltip2 = tooltip || tooltip == 'hover')}
          on:mouseleave={() => (display_tooltip2 = (!!tooltip && !!current_pointer) || (tooltip == 'hover' ? false : tooltip))}>
          <div key="k8" class="relative -mt-2 w-1">
            <Tooltip text={indLabel[1]} show={display_tooltip2} />
          </div>
        </div>
      {/if}
      <div key="k9" class="absolute text-gray-800 -ml-1 bottom-0 left-0 -mb-6">
        {minLabel}
      </div>
      <div
        key="k10"
        class="absolute text-gray-800 -mr-1 bottom-0 right-0 -mb-6">
        {maxLabel}
      </div>
    </div>
  </div>

</div>
