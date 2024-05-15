<script>
    import { onMount } from 'svelte';
    import Notification from '$lib/Notification.svelte';
    
    let phoneNumber = '';
    let message = '';
    let notificationMessage = '';
    let notificationType = 'success';
    let showNotification = false;

    async function makeCall() {
        try {
            const response = await fetch(`http://localhost:8080/api/call?to=${phoneNumber}&message=${message}`, {
                method: 'POST',
            });

            if (response.ok) {
                const data = await response.json();
                notificationMessage = data.message || 'Звонок успешно совершен!';
                notificationType = 'success';
            } else {
                const errorData = await response.json();
                notificationMessage = errorData.error || 'Ошибка при совершении звонка';
                notificationType = 'error';
            }
        } catch (error) {
            notificationMessage = 'Сетевая ошибка: ' + error.message;
            notificationType = 'error';
        } finally {
            showNotification = true;
            setTimeout(() => {
                showNotification = false;
            }, 3000); // Уведомление будет видно 3 секунды
        }
    }
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
    <button on:click={makeCall}>Совершить звонок</button>
</div>

{#if showNotification}
    <Notification message={notificationMessage} type={notificationType} />
{/if}
