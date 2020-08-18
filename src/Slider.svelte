<script lang="ts">
  import { onDestroy, beforeUpdate, afterUpdate } from "svelte";

  const items: string[] = [
    "https://ecs7-p.tokopedia.net/img/cache/750/NsjrJu/2020/8/17/cfb08963-1e67-4398-a290-2d824a845517.jpg.webp",
    "https://ecs7-p.tokopedia.net/img/cache/750/NsjrJu/2020/7/29/2a086572-3d1c-4af6-ab34-48355ed3850b.jpg.webp",
    "https://ecs7-p.tokopedia.net/img/cache/750/NsjrJu/2020/8/16/4817d04d-c974-454b-8567-e5ff42cd6f52.jpg.webp",
    "https://ecs7-p.tokopedia.net/img/cache/750/NsjrJu/2020/8/17/545987ce-da7a-4669-8285-b182c140da60.jpg.webp",
    "https://ecs7-p.tokopedia.net/img/cache/750/NsjrJu/2020/8/16/a43ec9db-3f7f-4e85-822e-651abe4f312a.jpg.webp",
    "https://ecs7-p.tokopedia.net/img/cache/750/NsjrJu/2020/8/16/69874b62-a952-424f-9454-819b789d815a.jpg.webp",
    "https://ecs7-p.tokopedia.net/img/cache/750/NsjrJu/2020/8/17/2083abf4-c006-47da-b3c6-7f4bdcbcdfaa.jpg.webp",
    "https://ecs7-p.tokopedia.net/img/cache/750/NsjrJu/2020/8/16/f0660d7c-9b33-46d4-8caf-83b29a516ebd.jpg.webp",
    "https://ecs7-p.tokopedia.net/img/cache/750/NsjrJu/2020/8/16/11a121cd-63af-471b-89c3-8d9a3424ebe5.jpg.webp",
    "https://ecs7-p.tokopedia.net/img/cache/750/NsjrJu/2020/8/17/7d610717-3618-495c-b5cc-a6fa6a7aba07.jpg.webp",
    "https://ecs7-p.tokopedia.net/img/cache/750/NsjrJu/2020/8/17/cfb08963-1e67-4398-a290-2d824a845517.jpg.webp",
    "https://ecs7-p.tokopedia.net/img/cache/750/NsjrJu/2020/7/29/2a086572-3d1c-4af6-ab34-48355ed3850b.jpg.webp",
    "https://ecs7-p.tokopedia.net/img/cache/750/NsjrJu/2020/8/16/4817d04d-c974-454b-8567-e5ff42cd6f52.jpg.webp",
    "https://ecs7-p.tokopedia.net/img/cache/750/NsjrJu/2020/8/17/545987ce-da7a-4669-8285-b182c140da60.jpg.webp",
  ];

  let translateX: number = -200;
  let index: number = 2;
  let interval: number = setInterval(nextSlide, 3000);
  let timer: number;
  let motion: string = "";

  function nextSlide() {
    translateX = translateX - 100;
    index++;
    motion = "motion";
  }

  function resetTimer() {
    clearInterval(interval);
    interval = setInterval(nextSlide, 3000);
  }

  onDestroy(() => clearInterval(interval));

  beforeUpdate(() => {
    clearTimeout(timer);
  });

  afterUpdate(() => {
    timer = setTimeout(() => {
      translateX = translateX == -1200 ? -200 : translateX;
      translateX = translateX == -100 ? -1100 : translateX;
      motion = "";
    }, 400);
    index = index == 10 ? 0 : index;
    index = index == -1 ? 9 : index;
  });

  function prev() {
    if (translateX < -100) {
      translateX += 100;
      index--;
      resetTimer();
      motion = "motion";
    }
  }

  function next() {
    if (translateX > -1200) {
      translateX -= 100;
      index++;
      resetTimer();
      motion = "motion";
    }
  }
</script>

<style type="text/scss">
  .wrapper {
    box-sizing: border-box;
    padding: 0 22%;
    width: 100%;
    min-height: 179px;
    display: flex;
    white-space: nowrap;
    height: fit-content;
    overflow: hidden;
    position: relative;

    img {
      width: 100%;
      flex-shrink: 0;
      height: auto;
      border-radius: 25px;
    }

    &.motion > img {
      transition: transform 0.4s ease-in-out;
    }

    .controls {
      width: 22%;
      height: 100%;
      top: 0;
      position: absolute;
      cursor: pointer;

      button {
        position: absolute;
        top: 50%;
        box-shadow: rgba(49, 53, 59, 0.12) 0px 1px 6px 0px;
        background-color: #ffff;
        opacity: 0;
        background-size: 24px;
        font-size: 0px;
        width: 32px;
        height: 32px;
        border-width: initial;
        border-style: none;
        border-color: initial;
        border-image: initial;
        outline: none;
        background-repeat: no-repeat;
        background-position: center center;
        border-radius: 50%;
        cursor: pointer;
        transition: all 0.5s ease 0s;
      }

      &.prev {
        left: 0;
      }

      &.next {
        right: 0;
      }

      .icon-prev {
        background-image: url(https://ecs7.tokopedia.net/assets-tokopedia-lite/v2/zeus/production/3d22d4aa.svg);
        transform: translateY(-50%) rotate(-90deg);
        left: 0;
      }

      .icon-next {
        background-image: url(https://ecs7.tokopedia.net/assets-tokopedia-lite/v2/zeus/production/3d22d4aa.svg);
        transform: translateY(-50%) rotate(90deg);
        right: 0;
      }
    }

    &:hover .controls button {
      opacity: 1;
    }

    &:hover .controls:nth-child(odd) button {
        left: 8px;
        right: auto;
    }

    &:hover .controls:nth-child(even) button {
        left: auto;
        right: 8px;
    }
  }
</style>

<div class="wrapper {motion}">
  {#each items as item}
    <img
      src={item}
      alt=""
      style="transform: translateX({translateX}%) {index == items.indexOf(item) ? 'scale(1)' : 'scale(0.9)'}" />
  {/each}
  <div class="controls prev" on:click={prev}>
    <button type="button" class="icons icon-prev">prev</button>
  </div>
  <div class="controls next" on:click={next}>
    <button type="button" class="icons icon-next">next</button>
  </div>
</div>
