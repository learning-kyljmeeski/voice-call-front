<script>
    import { onMount } from 'svelte';
    import Notification from '$lib/Notification.svelte';
    
    let phoneNumber = '';
    let message = '';
    let notificationMessage = '';
    let notificationType = 'success';
    let showNotification = false;
    let loading = false;
    let phoneNumberErrorText = ' ';
    let messageErrorText = ' ';

    function handlePhoneInput(event) {
        // Получаем значение из поля ввода
        const inputValue = event.target.value;
        
        // Обновляем переменную phoneNumber, добавляя к ней значение из поля ввода
        phoneNumber = '+996' + inputValue;
    }

    async function makeCall() {
        if (phoneNumber.length != 13) {
            phoneNumberErrorText = 'I need the real number';
            return;
        } else {
            phoneNumberErrorText = ' ';
        }

        if (message.trim().length === 0) {
            messageErrorText = 'Write something :)'
            return;
        } else {
            messageErrorText = ' ';
        }

        loading = true;

        try {
            const response = await fetch(`http://localhost:8080/api/call?to=${phoneNumber}&message=${message}`, {
                method: 'POST',
            });

            if (response.ok) {
                const data = await response.text();
                notificationMessage = 'The call was made';
                notificationType = 'success';
                console.log(data)
            } else {
                const errorData = await response.json();
                notificationMessage = errorData.error || 'Error when making a call';
                notificationType = 'error';
                console.error('Error when making a call:', errorData);
            }
        } catch (error) {
            notificationMessage = 'Network error: ' + error.message;
            notificationType = 'error';
            console.error('Network error:', error);
        } finally {
            loading = false;
            showNotification = true;
            setTimeout(() => {
                showNotification = false;
            }, 3000);
            message = '';
            phoneNumberErrorText = ' ';
            messageErrorText = ' '
        }
    }
</script>

<style>
    .container {
        max-width: 500px;
        margin: 0 auto;
        padding: 20px;
        text-align: center;
        font-family: "Courier New", Courier, monospace;
    }

    input, textarea {
        border-radius: 5px;
        border: 2px solid #ccc;
        outline: none;
    }

    .input-group {
        display: flex;
        flex-direction: column;
    }

    .input-group .phone-input-group {
        display: flex;
    }

    .input-group .phone-input-group input:first-child {
        width: 110px;
        text-align: center;
        border-top-right-radius: 0;
        border-bottom-right-radius: 0;
    }

    .input-group .phone-input-group input:nth-child(2) {
        border-top-left-radius: 0;
        border-bottom-left-radius: 0;
        border-left: 0;
    }

    .input-group span.error-message {
        min-height: 1.3em;
        margin-top: 5px;
        font-size: 12px;
        color:red;
        margin-right: auto;
        font-family: monospace;
    }

    label {
        display: block;
        margin-bottom: 5px;
        font-weight: bold;
    }

    input, textarea {
        font-family: "Courier New", Courier, monospace;
        width: 100%;
        padding: 10px;
        font-size: 16px;
        box-sizing: border-box;
    }

    button {
        margin-top: 15px;
        font-family: "Courier New", Courier, monospace;
        padding: 12px 20px;
        font-size: 16px;
        background-color: #007bff;
        color: white;
        border: none;
        cursor: pointer;
        width: 210px;
        border-radius: 5px;
    }

    button:hover {
        background-color: #0056b3;
    }
</style>

<div class="container">
    <div class="input-group">
        <label for="phone">Phone number</label>
        <div class="phone-input-group">
            <input type="text" value="🇰🇬 +996" disabled>
            <input type="tel" id="phone" on:input={handlePhoneInput} placeholder="708584859" autocomplete="off"/>
        </div>
        <span class="error-message">{phoneNumberErrorText}</span>
    </div>
    <div class="input-group">
        <label for="message">Message</label>
        <textarea id="message" bind:value={message} placeholder="Enter the message text"></textarea>
        <span class="error-message">{messageErrorText}</span>
    </div>
    <button on:click={makeCall} disabled={loading}> <!-- Добавим disabled для блокировки кнопки во время загрузки -->
        {#if loading}
            Working on it ...
        {:else}
            Call
        {/if}
    </button>
</div>

{#if showNotification}
    <Notification message={notificationMessage} type={notificationType} />
{/if}
