<script>
    import { fade } from 'svelte/transition';
    import { ss } from './state.svelte';
    import { post } from './utils';

    let status = $state('Send Message');
    let result = $state({});

    const handleSubmit = async (data) => {
        status = 'Sending...';
        const formData = new FormData(data.currentTarget);
        const object = Object.fromEntries(formData);
        const json = JSON.stringify(object);

        const response = await fetch('https://api.web3forms.com/submit', {
            method: 'POST',
            headers: { 'Content-Type': 'application/json', Accept: 'application/json' },
            body: json,
        });

        result = await response.json();

        if (result.success) {
            console.log(result);
            // status = result.message || 'Success!';
            status = 'Success!';
            post(() => (ss.feedback = false), 1000);
        }
    };
</script>

<form class="popup" onsubmit={handleSubmit} transition:fade={{ duration: 200 }}>
    <input type="hidden" name="access_key" value="d371bfe0-3e07-4c66-b930-72484e053b77" />
    <input type="hidden" name="app" value="Beautiful Mind Games" />
    <span class="label">Name</span>
    <input type="text" name="name" required />
    <span class="label">Email</span>
    <input type="email" name="email" required />
    <span class="label">Your Message</span>
    <textarea name="message" required></textarea>

    <div class="buttons">
        <button class="button" onclick={() => (ss.feedback = false)}>Cancel</button>
        <button class="button {result.success ? 'success' : ''}" type="submit">{status}</button>
    </div>
</form>

<style>
    .popup {
        grid-area: 1/1;
        width: calc(min(90dvw, 340px));
        display: grid;
        gap: 10px;
        place-self: end center;
        margin-bottom: 4em;
        z-index: 1;
        background: #2f0504;
        padding: 20px;
        border-radius: 5px;
        box-sizing: border-box;
        overflow: hidden;
    }

    .label {
        font-family: Poppins;
        font-size: 14px;
        color: #ffffffc0;
        margin-bottom: -5px;
    }

    input,
    textarea {
        box-sizing: border-box;
        border-radius: 5px;
        border: 1px solid #d1d5db;
        font-family: Poppins;
        font-size: 15px;
        padding: 5px 10px;
        resize: vertical;
        -webkit-transition: 0.1s;
        transition: 0.1s;
        outline: none;
        width: 100%;
    }

    input {
        height: 34px;
    }

    textarea {
        min-height: 100px;
        max-height: 50dvh;
    }

    input:focus,
    textarea:focus {
        border: 2px solid #6366f1;
    }

    button {
        border-radius: 5px;
        min-height: 40px;
        background: #6366f1;
        color: #fff;
        border: none;
        font-family: Poppins;
        font-size: 15px;
        padding: 5px 10px;
        cursor: pointer;
        transition: background-color 0.3s;
        margin-top: 10px;
    }

    button:hover {
        filter: contrast(1.3) saturate(1.3);
    }

    .success {
        background: #059624;
    }

    .buttons {
        display: grid;
        gap: 10px;
        grid-template-columns: 0.7fr 1fr;
    }
</style>
