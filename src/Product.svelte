<script lang="ts">
    import { afterUpdate, beforeUpdate } from "svelte";

    import { index } from "./stores/index";

    const images = [
        "https://storage.googleapis.com/hijup-production-sg-core/system/product_image/image/373935/smaller_ALMAS_Pashmina-Dark_Olive_1.jpg",
        "https://storage.googleapis.com/hijup-production-sg-core/system/product_image/image/373936/big_ALMAS_Pashmina-Dark_Olive_2.jpg",
        "https://storage.googleapis.com/hijup-production-sg-core/system/product_image/image/373937/big_ALMAS_Pashmina-Dark_Olive_3.jpg",
    ];
    const length = images.length;
    const divider = length / 2;
    let tx = 0;
    let time = 0.25;
    let isFromPortal = false
    let isDisableButton = false

    const cloneItems =
        length % 2 === 0
            ? [
                  ...images.slice(divider, length),
                  ...images,
                  ...images.slice(0, divider),
              ]
            : [
                  ...images.slice(Math.floor(divider), length),
                  ...images,
                  ...images.slice(0, Math.ceil(divider)),
              ];

    $: {
        const firstPosition = Math.floor(divider) * 150;

        tx = $index > 0 ? firstPosition + 150 * $index : firstPosition;
    }

    const handleNext = () => {
        index.update((i) => i + 1);
        isDisableButton = true
    };

    const handlePrev = () => {
        index.update((i) => i - 1)
        isDisableButton = true
    }

    beforeUpdate(() => {
        if (isFromPortal) {
            time = 0;
        } else {
            time = 0.25
        }
    });

    afterUpdate(() => {
        isFromPortal = false
    })

    const handleTransitionEnd = () => {
        if ($index == length + 1) {
            index.set(1);
            isFromPortal = true
        } else if ($index == 0) {
            index.set(length)
            isFromPortal = true
        }
        isDisableButton = false
    };
</script>

<div class="container">
    <button on:click={handlePrev} disabled={isDisableButton}>Prev</button>
    <div class="box">
        <div
            class="slider"
            style={`transform: translate3d(-${tx}px, 0px, 0px);
                    transition: all ${time}s ease 0s;`}
            on:transitionend={handleTransitionEnd}
        >
            {#each cloneItems as image, i}
                <img src={image} alt={`product-${i}`} />
            {/each}
        </div>
    </div>
    <button on:click={handleNext} disabled={isDisableButton}>Next</button>
</div>

<style>
    * {
        padding: 0;
        margin: 0;
        box-sizing: border-box;
    }

    .container {
        display: flex;
        justify-content: center;
        align-items: center;
        width: 100%;
        height: 100vh;
    }

    .box {
        width: 150px;
        height: 15em;
        overflow-x: hidden;
        background-color: bisque;
    }

    .slider {
        width: 100%;
        height: 100%;
        display: flex;
    }
</style>
