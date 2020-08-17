<script lang="ts">
    import { onDestroy, afterUpdate } from 'svelte'

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
    ]

    let translateX: number = -200
    let index: number = 2
    const interval: number = setInterval(() => {
        translateX = translateX - 100
        index++
    }, 2000)
    onDestroy(() => clearInterval(interval))

    afterUpdate(() => {
        setTimeout(() => {
            translateX = translateX == -1200 ? -200 : translateX
            index = index == 10 ? 0 : index
        }, 1000)
    })

    $: console.log(index)
</script>

<div class="wrapper">
    {#each items as item}
        <img 
            src={item} 
            alt=""
            style="transform: translateX({translateX}%) {index == items.indexOf(item) ? 'scale(1)' : 'scale(0.9)'}"
            class="{translateX == -200 ? 'reset' : ''}">
    {/each}
</div>

<style type="text/scss">
    .wrapper {
        box-sizing: border-box;
        padding: 0 22%;
        width: 100%;
        min-height: 179px;
        display: flex;
        white-space: nowrap;
        margin: auto;
        overflow: hidden;

        img {
            flex-shrink: 0;
            height: auto;
            border-radius: 25px;
            transition: transform 0.4s ease-in-out;

            &.reset {
                transition: none;
            }
        }
    }
</style>