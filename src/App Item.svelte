<script>
    import { APPS } from './const';
    import { ss } from './state.svelte';

    const { name, index } = $props();
    const app = APPS[name];
    const { icon, url, desc } = app;
    const flipped = $derived(ss.flip === index + 1);
    const rotateName = $derived(`rotateX(${flipped ? 90 : 0}deg)`);
    const rotateDesc = $derived(`rotateX(${flipped ? 0 : 90}deg)`);
    const delay = $derived(flipped ? 0 : 0.3);
    let gridArea = $state();
    let maxWidth = $state();

    $effect(() => {
        const onResize = () => {
            const r = document.body.getBoundingClientRect();
            const wide = r.width > 600;
            const col = wide ? (index % 2) + 1 : 1;
            maxWidth = ss.flip && (!wide || ((ss.flip - 1) % 2) + 1 === col) ? '200px' : '155px';

            if (wide) {
                const row = Math.floor(index / 2) + 1;
                gridArea = `${row}/${col}`;
            } else {
                gridArea = `${index + 1}/1`;
            }
        };

        onResize();

        window.addEventListener('resize', onResize);
        return () => window.removeEventListener('resize', onResize);
    });

    const onPointerDown = (e) => {
        if (e.target.tagName !== 'IMG') {
            ss.flip = flipped ? null : index + 1;
        }
    };
</script>

<div class="app-item" style="grid-area: {gridArea};" onpointerdown={onPointerDown}>
    <a tabindex="-1" href={url} target="_blank" rel="noopener noreferrer" style="display: grid">
        <img src={icon} alt={name} width={64} style="background-image: radial-gradient(black, transparent 120%)" />
    </a>
    <div class="app-info" style="max-width: {maxWidth};">
        <div class="app-name" style="grid-area: 1/1; transform: {rotateName}; transition-delay: {delay}s;">
            {name}
        </div>
        <div class="app-desc" style="transform: {rotateDesc}; transition-delay: {0.3 - delay}s">
            {desc}
        </div>
    </div>
</div>

<style>
    .app-item {
        display: grid;
        gap: 20px;
        grid: auto / auto 1fr;
        grid-auto-flow: column;
        place-content: center;
        align-items: center;
        padding-right: 20px;
        background-image: linear-gradient(to right, transparent, black 180%);
        height: 64px;
        cursor: pointer;
        margin: 0 10px;
    }

    .app-info {
        display: grid;
        align-items: center;
        overflow: hidden;
        transition: max-width 0.3s;
    }

    .app-name,
    .app-desc {
        transition-duration: 0.3s;
    }

    .app-desc {
        grid-area: 1/1;
        font-size: 14px;
        width: 200px;
    }
</style>
