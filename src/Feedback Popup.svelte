<script>
    const validate = () => {
        'use strict';
        /*
         * Form Validation
         */

        // Fetch all the forms we want to apply custom validation styles to
        const forms = document.querySelectorAll('.needs-validation');
        const result = document.getElementById('result');
        // Loop over them and prevent submission
        Array.prototype.slice.call(forms).forEach(function (form) {
            form.addEventListener(
                'submit',
                (event) => {
                    if (!form.checkValidity()) {
                        event.preventDefault();
                        event.stopPropagation();

                        form.querySelectorAll(':invalid')[0].focus();
                    } else {
                        /*
                         * Form Submission using fetch()
                         */

                        const formData = new FormData(form);
                        event.preventDefault();
                        event.stopPropagation();
                        const object = {};
                        formData.forEach((value, key) => {
                            object[key] = value;
                        });
                        const json = JSON.stringify(object);
                        result.innerHTML = 'Please wait...';

                        fetch('https://api.web3forms.com/submit', {
                            method: 'POST',
                            headers: { 'Content-Type': 'application/json', Accept: 'application/json' },
                            body: json,
                        })
                            .then(async (response) => {
                                let json = await response.json();
                                if (response.status == 200) {
                                    result.innerHTML = json.message;
                                    result.classList.remove('text-gray-500');
                                    result.classList.add('text-green-500');
                                } else {
                                    console.log(response);
                                    result.innerHTML = json.message;
                                    result.classList.remove('text-gray-500');
                                    result.classList.add('text-red-500');
                                }
                            })
                            .catch((error) => {
                                console.log(error);
                                result.innerHTML = 'Something went wrong!';
                            })
                            .then(function () {
                                form.reset();
                                form.classList.remove('was-validated');
                                setTimeout(() => {
                                    result.style.display = 'none';
                                }, 5000);
                            });
                    }
                    form.classList.add('was-validated');
                },
                false,
            );
        });
    };

    $effect(() => {
        validate();
    });
</script>

<!-- <div class="popup"></div> -->

<form class="popup" action="https://api.web3forms.com/submit" method="POST">
    <input type="hidden" name="access_key" value="YOUR_ACCESS_KEY_HERE" />
    <span class="label">Name</span>
    <input class="input" type="text" name="name" required />
    <span class="label">Email</span>
    <input class="input" type="email" name="email" required />
    <span class="label">Your Message</span>
    <textarea class="textarea" name="message" required></textarea>
    <input type="hidden" name="redirect" value="https://web3forms.com/success" />

    <button type="submit">Send Message</button>
</form>

<style>
    .popup {
        grid-area: 1/1;
        width: 320px;
        display: grid;
        gap: 10px;
        place-self: end center;
        margin-bottom: 4em;
        z-index: 1;
        background: #fffffff0;
        padding: 20px;
        border-radius: 5px;
        box-sizing: border-box;
    }

    .label {
        font-family: Poppins;
        font-size: 14px;
        color: #000;
        margin-bottom: -5px;
    }

    .input,
    .textarea {
        border-radius: 5px;
        border: 1px solid #d1d5db;
        min-height: 24px;
        font-family: Poppins;
        font-size: 15px;
        padding: 5px 10px;
        resize: vertical;
    }

    .textarea {
        min-height: 100px;
        max-height: 50dvh;
    }
</style>
