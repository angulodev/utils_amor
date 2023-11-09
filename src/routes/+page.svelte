<script>
    let color1 = "#ff8080";
    let color2 = "#ff0000";
    let iteraciones = 5;
    /**
     * @type {any[]}
     */
    let gradientColors = [];

    function generarGradiente() {
        gradientColors = [];
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
            const rgb = `rgb(${r},${g},${b})`;
            const hex = `#${((r << 16) | (g << 8) | b)
                .toString(16)
                .padStart(6, "0")}`;
            gradientColors.push({ rgb, hex });
        }
    }
    generarGradiente();
</script>

<main>
    <header class="flex flex-col m-4">
        <h2 class="mb-2">Selección de colores e iteraciones</h2>
        <div>
            <label for="color1">Seleccione Color 1:</label>
            <input
                class="border border-slate-300 rounded-md"
                type="color"
                id="color1"
                bind:value={color1}
                on:change={generarGradiente}
            />
        </div>
        <div>
            <label for="color2">Seleccione Color 2:</label>
            <input
                class="border border-slate-300 rounded-md"
                type="color"
                id="color2"
                bind:value={color2}
                on:change={generarGradiente}
            />
        </div>
        <div>
            <label for="iteraciones">Iteraciones (0-10):</label>
            <input
                class="border border-slate-300 rounded-md"
                type="number"
                id="iteraciones"
                bind:value={iteraciones}
                min="0"
                max="10"
                on:change={generarGradiente}
            />
        </div>
    </header>

    <div class="flex-wrap">
        {#each gradientColors as { rgb, hex }}
            <div
                class="w-26 h-26 m-1 shadow-md justify-center text-center"
                style="background-color: {rgb}"
            >
                <p>{rgb}</p>
                <p>{hex}</p>
            </div>
        {/each}
    </div>
</main>
