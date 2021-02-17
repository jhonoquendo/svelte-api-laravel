<script>
    import { createEventDispatcher } from 'svelte';
    import Error from './Error.svelte';
    const dispatch = createEventDispatcher();
    export let proceso;
    //let urlBase = "http://restapi.test/"
	let urlBase = "https://bancoapi-oquendo.herokuapp.com/"
    let nombre='';
    let cantidad='';
    let error = false;
    let disabled = false;

    const validar = () => {
        if(nombre.trim() === '' || cantidad <= 0 ){
            error = true;
            return;
        }
        error = false;
        return;
    }

    const limpiar = () => {
        nombre = '';
        cantidad = '';
        error = false;
    }
    
    const ejecutar = async (e) => {
        e.preventDefault();
        validar();
        if(error == false){
            disabled = true;
            setTimeout( async ()=>{
                cantidad = parseInt(cantidad);
                let data = new FormData();
                data.append('type',proceso);
                data.append('name',nombre);
                data.append('destination','0');
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
                disabled = false;
            },1000);
            
        }
    }
</script>

<form on:submit={ejecutar} id="form-deposito">
    {#if error}
        <Error mensaje="Los campos son obligatorios"/>
    {/if}
    <div class="campo">
        <label for="nombre">Nombre de Propietario</label>
        <input
            id="nombre"
            name="name"
            type="text"
            class="u-full-width"
            placeholder="Nombre de propietario"
            bind:value={nombre}
        />
    </div>
    <div class="campo">
        <label for="cantidad">Cantidad de deposito</label>
        <input
            id="cantidad"
            name="amount"
            type="number"
            class="u-full-width"
            placeholder="Ej. 300"
            bind:value={cantidad}
        />
    </div>
    <input
        type="submit"
        class="button-primary u-full-width"
        value="Agregar {proceso}"
        disabled = {disabled}
        />
</form>