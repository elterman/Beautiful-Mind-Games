<script>
    import { Motion } from 'svelte-motion';
    import AppItem from './App Item.svelte';
    import { APPS } from './const';
    import { ss } from './state.svelte';
    import { scrollClass } from './utils';

    let wy = $state(0);

    $effect(() => {
        const onResize = () => {
            const r = document.body.getBoundingClientRect();
            wy = r.height;
        };

        onResize();

        window.addEventListener('resize', onResize);
        return () => window.removeEventListener('resize', onResize);
    });
</script>

<div class="app-page">
    <!-- <div className='content' style={{ height: wy }}>
        <div className='apps-container'>
            <div className={`apps ${scrollClass()}`} style={{ maxHeight: wy - 100 }}>
                {_.map(_.keys(APPS), (key, i) => <AppItem key={key} name={key} index={i} wx={wx} />)}
            </div>
        </div>
        <motion.div className='hint' animate={{ opacity: flip ? 1 : 0, transform: `scale(${flip ? 1 : 0})` }}
            transition={{ type: 'spring', damping: 10, delay: flip ? 0.75 : 0 }}>
            Click on the app icon to play.
        </motion.div>
        <a href="mailto:bmgomg@gmail.com?subject=Beautiful Mind Games" target="_blank" rel="noopener noreferrer"
            className='mail'>✉️</a>
    </div> -->
    <div class="content" style="height: {wy}">
        <div class="apps-container">
            <div class="apps {scrollClass()}" style="max-height: {wy - 100}px;">
                {#each Object.keys(APPS) as key, i (i)}
                    <AppItem name={key} index={i} />
                {/each}
            </div>
        </div>
        <Motion
            animate={{ opacity: ss.flip ? 1 : 0, transform: `scale(${ss.flip ? 1 : 0})` }}
            transition={{ type: 'spring', damping: 10, delay: ss.flip ? 0.75 : 0 }}
            let:motion>
            <div class="hint" use:motion>Click on the app icon to play.</div>
        </Motion>
    </div>
</div>

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
