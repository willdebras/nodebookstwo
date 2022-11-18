<script>
    export let itemFunc = () => {}
</script>

<div>
    <button on:click={itemFunc}>Add a Node</button>
</div>

<style>
    div {
        position:absolute;
        width:300px;
        height:calc(100% - 90px);
        top:25px;
        right:25px;
        border:1px solid black;
        padding:20px;

    }
</style>