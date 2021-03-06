<script>
  import rAF from './rAF';
  import { onMount, createEventDispatcher } from 'svelte';
  let swipeItemInner;
  export let classes = '', track_height = false, active;

  const dispatch = createEventDispatcher();

  onMount(() => {
    setTimeout(() => {
      pollForRender()
    }, 100)
  });

  $: if (track_height && active) {
		rAF(() => {
      fireSizeChange()
    });
	}

  function pollForRender(){
    if(swipeItemInner && typeof swipeItemInner != 'undefined'){
      init();
    }else{
      setTimeout(() => {
        pollForRender();
      }, 200);
    }
  }

  function init(){
    fireSizeChange();
  }

  function fireSizeChange(){
    if(track_height){
      if(!swipeItemInner || typeof swipeItemInner == 'undefined'){
        pollForRender();
      }else{
        rAF(() => {
          if(!swipeItemInner || typeof swipeItemInner == 'undefined'){
            return;
          }
          let h1 = swipeItemInner.scrollHeight,
          h2 = swipeItemInner.clientHeight;
          dispatch("heightChange", {
            height: Math.max(h1, h2)
          });
        });
      }
    }
  }




</script>

<style>
  .swipeable-item {
    position: absolute;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    transition-timing-function: ease-out;
  }
</style>

<div bind:this={swipeItemInner} class="swipeable-item {classes}">
  <slot/>
</div>