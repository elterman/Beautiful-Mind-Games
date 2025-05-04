<script>
    let wx = $state(0);

    $effect(() => {
        const onResize = () => {
            wx = document.body.getBoundingClientRect();
        };

        onResize();

        window.addEventListener('resize', onResize);
        return () => window.removeEventListener('resize', onResize);
    });
</script>

<div class="app-page"></div>

<style>
    .app-page {
        display: grid;
        height: 100dvh;
        width: 100dvw;
        font-family: Roboto Condensed;
        font-size: 16.5px;
        user-select: none;
        -webkit-tap-highlight-color: #0000;
        color: #ffffffa0;
        background: #8a0000;
        background-image: radial-gradient(transparent, black 100%), url('$lib/images/Pattern.webp');
        box-sizing: border-box;
    }

    .content {
        display: grid;
    }

    .apps-container {
        grid-area: 1/1;
        place-self: center;
        display: grid;
    }

    .apps {
        place-content: start center;
        display: grid;
        gap: 10px 0;
        text-shadow: 1px 1px 1px black;
    }

    .hint {
        grid-area: 1/1;
        place-self: end center;
        font-size: 16px;
        margin-bottom: 1em;
        text-shadow: 1px 1px 1px black;
        color: white;
    }
</style>
