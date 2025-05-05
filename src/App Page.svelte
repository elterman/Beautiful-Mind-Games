<script>
    import { Motion } from 'svelte-motion';
    import AppItem from './App Item.svelte';
    import { APPS } from './const';
    import { ss } from './state.svelte';
    import { scrollClass, tapOrClick, windowSize } from './utils';
    import Feedback from '$lib/images/Feedback.webp';
    import FeedbackPopup from './Feedback Popup.svelte';

    let wy = $state(0);

    $effect(() => {
        const onResize = () => {
            const { y } = windowSize();
            wy = y;
        };

        onResize();

        window.addEventListener('resize', onResize);
        return () => window.removeEventListener('resize', onResize);
    });
</script>

<div class="app-page">
    <div class="content {ss.feedback ? 'disabled' : ''}" style="height: {wy}">
        <div class="apps-container">
            <div class="apps {scrollClass()}" style="max-height: {wy - 125}px;">
                {#each Object.keys(APPS) as key, i (i)}
                    <AppItem name={key} index={i} />
                {/each}
            </div>
        </div>
        <Motion
            animate={{ opacity: ss.flip ? 1 : 0, transform: `scale(${ss.flip ? 1 : 0})` }}
            transition={{ type: 'spring', damping: 10, delay: ss.flip ? 0.75 : 0 }}
            let:motion>
            <div class="hint" use:motion>{`${tapOrClick()} app icon to play.`}</div>
        </Motion>
        <div class="feedback {ss.feedback ? 'disabled' : ''}" onpointerdown={() => (ss.feedback = true)}>
            <img src={Feedback} alt="" width={30} />
        </div>
    </div>
    {#if ss.feedback}
        <FeedbackPopup />
    {/if}
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
        grid-area: 1/1;
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
        place-self: start center;
        font-size: 16px;
        margin-top: 1.35em;
        text-shadow: 1px 1px 1px black;
        color: white;
    }

    .feedback {
        grid-area: 1/1;
        place-self: end center;
        margin-bottom: 1em;
        opacity: 0.8;
        cursor: pointer;
    }

    .feedback:hover {
        opacity: 1;
    }

    .disabled {
        pointer-events: none;
        opacity: 0.5;
    }
</style>
