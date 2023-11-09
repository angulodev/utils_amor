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

    let mensaje = "";
    let posX = 0;
    let posY = 0;
    /**
     * @param {string} hexValue
     */
    function copiarAlPortapapeles(hexValue, event) {
        navigator.clipboard
            .writeText(hexValue)
            .then(() => {
                // console.log(`Copiado: ${hexValue}`);
                mostrarMensaje(event, hexValue);
            })
            .catch((err) => {
                console.error("Error al copiar al portapapeles: ", err);
            });
    }

    function mostrarMensaje(event, hexValue) {
        posX = event.clientX;
        posY = event.clientY;
        mensaje = hexValue;
        setTimeout(() => {
            mensaje = "";
        }, 3000);
    }
    generarGradiente();
</script>

<main>
    <header class="flex flex-col m-4">
        <h2 class="mb-2">Generador de gradientes</h2>
        <h3 class="mb-2">Selección de colores e iteraciones</h3>
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

    <div class="p-4 grid md:flex md:flex-wrap gap-4">
        {#each gradientColors as { rgb, hex }}
            <div
                class="relative bg-gray-200 p-4 rounded-md w-full md:w-auto md:h-auto
                text-center
                shadow-lg cursor-pointer"
                style="background-color: {rgb}"
                on:click={(event) => copiarAlPortapapeles(hex, event)}
                on:keydown={(event) => {
                    if (event.key === "Enter" || event.key === " ") {
                        copiarAlPortapapeles(hex, event);
                    }
                }}
                tabindex="0"
            >
                <p>{rgb}</p>
                <p>{hex}</p>
            </div>
        {/each}
    </div>

    {#if mensaje}
        <div class="absolute" style="top: {posY}px; left: {posX}px;">
            <p
                class="bg-white border border-black p-2 rounded-md shadow-md text-center"
            >
                Color Copiado : <small class="text-lg" style="color:{mensaje}">
                    {mensaje}
                </small>
                😎💕
            </p>
        </div>
    {/if}
</main>
