<div class="image" style={"width: " +width+"px; height: " +height+"px;"}>
    <img  width="{width}" height="{height}" src="{source}" class="{loaded ? 'loaded' : ''}" alt="" on:load="{onLoaded}">
</div>

<script lang="ts">
    export let source : string;

    export let width : number;
    export let height : number;

    let loaded = false;
    
    function onLoaded() {
        loaded = true;
    }
</script>

<style lang="scss">
    .image {
        background-color: gray;
    }

    img {
        opacity: 0;
        transition: opacity 500ms ease;
        margin: 0px;
        box-shadow: 0 0 100px 10px rgba(0, 0, 0, 0.25);
          border-radius: 10px;

        &.loaded {
            opacity: 1;
        }
    }
</style>