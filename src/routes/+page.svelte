<script>
    let color1 = "#ff0000";
    let color2 = "#0000ff";
    let iteraciones = 5;
    /**
     * @type {any[]}
     */
    let gradientColors = [];

    function generarGradiente() {
        gradientColors = [];
        for (let i = 0; i < iteraciones; i++) {
            const r = Math.round(
                (parseInt(color1.slice(1, 3), 16) * (iteraciones - i) +
                    parseInt(color2.slice(1, 3), 16) * i) /
                    iteraciones
            );
            const g = Math.round(
                (parseInt(color1.slice(3, 5), 16) * (iteraciones - i) +
                    parseInt(color2.slice(3, 5), 16) * i) /
                    iteraciones
            );
            const b = Math.round(
                (parseInt(color1.slice(5), 16) * (iteraciones - i) +
                    parseInt(color2.slice(5), 16) * i) /
                    iteraciones
            );
            gradientColors.push(`rgb(${r},${g},${b})`);
        }
    }
</script>

<div>
    <label for="color1">Color 1:</label>
    <input
        type="color"
        id="color1"
        bind:value={color1}
        on:change={generarGradiente}
    />

    <label for="color2">Color 2:</label>
    <input
        type="color"
        id="color2"
        bind:value={color2}
        on:change={generarGradiente}
    />

    <label for="iteraciones">Iteraciones (0-10):</label>
    <input
        type="number"
        id="iteraciones"
        bind:value={iteraciones}
        min="0"
        max="10"
        on:change={generarGradiente}
    />

    <div class="gradient-box">
        {#each gradientColors as color}
            <div class="color-box" style="background-color: {color}" />
        {/each}
    </div>
</div>

<style>
    .gradient-box {
        display: flex;
        flex-wrap: wrap;
    }

    .color-box {
        width: 50px;
        height: 50px;
        margin: 5px;
        border: 1px solid #000;
    }
</style>
