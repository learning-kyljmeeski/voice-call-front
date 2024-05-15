<script>
    export let message = '';
    export let type = 'success'; // success, error

    let visible = true;

    function close() {
        visible = false;
    }
</script>

<style>
    .notification {
        position: fixed;
        top: 20px;
        right: 20px;
        padding: 15px 20px;
        background-color: var(--background-color);
        color: white;
        border-radius: 5px;
        box-shadow: 0 2px 5px rgba(0, 0, 0, 0.3);
        z-index: 1000;
    }
    .success {
        --background-color: #28a745;
    }
    .error {
        --background-color: #dc3545;
    }
    .close {
        margin-left: 15px;
        cursor: pointer;
    }
</style>

{#if visible}
    <div class="notification {type}">
        {message}
        <span class="close" on:click={close}>✖</span>
    </div>
{/if}
