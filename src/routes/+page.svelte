<script>
    import AppPage from '../App Page.svelte';
    import Splash from '../Splash.svelte';
    import { post } from '../utils';

    $effect(() => {
        const disable = (e) => {
            e.preventDefault();
        };

        window.addEventListener('contextmenu', disable);
        window.addEventListener('dblclick', disable);

        return () => {
            window.removeEventListener('contextmenu', disable);
            window.removeEventListener('dblclick', disable);
        };
    });

    let splash = $state(true);
    post(() => (splash = false), 2000);
</script>

<div class="app">
    <div class="vignette"></div>
    <AppPage/>

    {#if splash}
        <Splash />
    {/if}
</div>

<style>
    :global {
        body {
            margin: 0;
            overflow: hidden;
        }

        .no-highlight {
            -webkit-tap-highlight-color: transparent;
            -webkit-touch-callout: none;
            -webkit-user-select: none;
            -khtml-user-select: none;
            -moz-user-select: none;
            -ms-user-select: none;
            user-select: none;
        }

        .root-scroll {
            display: grid;
            overflow: auto;
            -webkit-overflow-scrolling: touch;
        }

        .root-scroll::-webkit-scrollbar {
            width: 10px;
            height: 10px;
        }

        .root-scroll::-webkit-scrollbar-thumb:hover {
            background: #00000058;
            /* background: #8a0000d0; */
        }

        .root-scroll::-webkit-scrollbar-thumb {
            border-radius: 50vw;
            background: #00000048;
            /* background: #8a0000b0; */
        }

        /* .root-scroll::-webkit-scrollbar-track {
        background: #fff2;
        } */

        .root-scroll::-webkit-scrollbar-track:hover {
            background: #ffffff05;
        }

        .root-scroll-mobile::-webkit-scrollbar {
            width: 2px;
            height: 2px;
        }

        .root-scroll-mobile::-webkit-scrollbar-thumb {
            border-radius: 1px;
        }
    }

    .app {
        display: grid;
        height: 100%;
        width: 100%;
        font-family: Roboto Condensed;
        font-size: 16.5px;
        user-select: none;
        -webkit-tap-highlight-color: #0000;
        color: #ffffffa0;
        background: #8a0000;
        box-sizing: border-box;
    }

    .vignette {
        grid-area: 1/1;
        background-image: radial-gradient(transparent, black 150%);
    }

    @font-face {
        font-family: Poppins;
        src: url('$lib/fonts/Poppins.ttf');
    }

    @font-face {
        font-family: 'Roboto Condensed';
        src: url('$lib/fonts/RobotoCondensed-Medium.ttf');
    }
</style>
