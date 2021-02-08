<script>
    import { createEventDispatcher } from 'svelte';
    import Error from './Error.svelte';
    const dispatch = createEventDispatcher();
    export let proceso;
    //let urlBase = "http://restapi.test/"
	let urlBase = "https://bancoapi-oquendo.herokuapp.com/"
    let origen='';
    let destino='';
    let cantidad='';
    let error = false;

    const validar = () => {
        if(origen <= 0 || destino <= 0 || cantidad <= 0 || origen == '' || destino == '' || cantidad == ''){
            error = true;
            return;
        }
        error = false;
        return;
    }

    const limpiar = () => {
        origen='';
        destino='';
        cantidad='';
        error = false;
    }

    const ejecutar = async (e) => {
        e.preventDefault();
        validar();
        if(error == false){
            cantidad = parseInt(cantidad);
            let data = new FormData();
            data.append('type',proceso);
            data.append('origin',origen);
            data.append('amount',cantidad);
            data.append('destination',destino);
            await fetch(`${urlBase}event`,{
                method:'POST',
                body: data
            })
            .then((res) => res.json())
                .then((res) => {
                    limpiar();
                    dispatch('ejecutar');
                });
        }
    }
</script>

<form on:submit={ejecutar}>
    {#if error}
        <Error mensaje="Los campos son obligatorios"/>
    {/if}
    <div class="campo">
        <label for="origen">Codigo de Cuenta de Origen</label>
        <input
            id="origen"
            type="text"
            class="u-full-width"
            placeholder="Nombre de propietario"
            bind:value={origen}
        />
    </div>
    <div class="campo">
        <label for="destino">Codigo de Cuenta de Destino</label>
        <input
            id="destino"
            type="text"
            class="u-full-width"
            placeholder="Nombre de propietario"
            bind:value={destino}
        />
    </div>
    <div class="campo">
        <label for="cantidad">Cantidad de Transferencia</label>
        <input
            id="cantidad"
            type="number"
            class="u-full-width"
            placeholder="Ej. 300"
            bind:value={cantidad}
        />
    </div>
    <input
        type="submit"
        class="button-primary u-full-width"
        value="Procesar {proceso}"
        />
</form>