<script>
    import { createEventDispatcher } from 'svelte';
    import Error from './Error.svelte';
    const dispatch = createEventDispatcher();
    export let proceso;
    //let urlBase = "http://restapi.test/"
	let urlBase = "https://bancoapi-oquendo.herokuapp.com/"
    let cuenta='';
    let cantidad='';
    let error = false;

    const validar = () => {
        if(cuenta <= 0 || cantidad <= 0 || cuenta == '' || cantidad == ''){
            error = true;
            return;
        }
        error = false;
        return;
    }

    const limpiar = () => {
        cuenta='';
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
            data.append('origin',cuenta);
            data.append('amount',cantidad);
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
        <label for="cuenta">Numero de Cuenta</label>
        <input
            id="cuenta"
            type="number"
            class="u-full-width"
            placeholder="Numero de cuenta"
            bind:value={cuenta}
        />
    </div>
    <div class="campo">
        <label for="cantidad">Cantidad de retiro</label>
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