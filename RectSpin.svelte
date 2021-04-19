<script lang="ts">
    import {onDestroy, onMount} from "svelte";

    export let background = `rgba(0, 0, 0, 0.95)`
    export let color = "white"
    export let speed = 30;

    let spinner: HTMLDivElement
    let interval

    let dot = {x: 0, y: 0}
    let dots = []

    onMount(() => {
        let rect = spinner.getBoundingClientRect()
        dot.x = rect.x
        dot.y = rect.y

        interval = setInterval(
            () => {
                if (dot.x < rect.x + rect.width && (dot.y < rect.y + rect.height) && (dot.y <= rect.y))
                    dot.x += speed
                else if (dot.y < rect.y + rect.height && dot.x >= rect.x + rect.width)
                    dot.y += speed
                else if (dot.x > rect.x)
                    dot.x -= speed
                else if ((dot.x < rect.x + rect.width))
                    dot.y -= speed
                dots = [{...dot}, ...dots]
                dots = dots.slice(0, 50)
            }, 16.6
        )
    })

    onDestroy(() => clearInterval(interval))
</script>

<div class="container" style="background: {background}">
    <div bind:this={spinner} class="content">
        <slot/>
    </div>
</div>

<div class="container">
    <svg class="spinner" width="100%" height="100%">
        {#each dots as item,index}
            <rect fill={color} opacity={1-index/dots.length} x={item.x} y={item.y} width="10" height="10"/>
        {/each}
    </svg>
</div>

<style>
    .container {
        width: 100%;
        height: 100%;
        position: fixed;
        left: 0;
        top: 0;
        display: flex;
        align-items: center;
    }

    svg {

    }

    .content {
        width: 300px;
        height: 200px;
        /*border: 1px solid red;*/
        /*outline: 1px solid blue;*/
        margin: auto;
        display: flex;
        align-items: center;
        justify-content: center;
        font-family: Verdana, serif;
        font-size: xx-large;
        color: white;
    }

    .spinner {
        /*border: 3px solid red;*/
        position: fixed;
        top: 0;
        left: 0;
    }
</style>
