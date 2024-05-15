<script>
    let phoneNumber = '';
    let message = '';
</script>

<style>
    .container {
        max-width: 500px;
        margin: 0 auto;
        padding: 20px;
        text-align: center;
        font-family: Arial, sans-serif;
    }

    .input-group {
        margin-bottom: 15px;
    }

    label {
        display: block;
        margin-bottom: 5px;
        font-weight: bold;
    }

    input, textarea {
        width: 100%;
        padding: 10px;
        font-size: 16px;
        box-sizing: border-box;
    }

    button {
        padding: 10px 20px;
        font-size: 16px;
        background-color: #007bff;
        color: white;
        border: none;
        cursor: pointer;
    }

    button:hover {
        background-color: #0056b3;
    }
</style>

<div class="container">
    <div class="input-group">
        <label for="phone">Номер телефона</label>
        <input type="tel" id="phone" bind:value={phoneNumber} placeholder="Введите номер телефона" />
    </div>
    <div class="input-group">
        <label for="message">Сообщение</label>
        <textarea id="message" bind:value={message} placeholder="Введите текст сообщения"></textarea>
    </div>
    <button on:click="{() => alert('Звонок совершен!')}">Совершить звонок</button>
</div>
