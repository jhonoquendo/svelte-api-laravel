<script>
    import { createEventDispatcher } from 'svelte';
    const dispatch = createEventDispatcher();
    let proceso = 'deposito';

    import Deposito from './Deposito.svelte';
    import Retiro from './Retiro.svelte';
    import Transferencia from './Transferencia.svelte';

    const enviar = () => {
        dispatch('enviar');
    }

    const ejecutar = () => {
        enviar();
    }
    
    const elegirProceso = () => {
        //console.log(proceso);
    }
</script>

<h2>Realiza tus procesos</h2>
<!-- <button class="button-primary u-full-width" on:click={enviar}>Probar</button> -->
<select name="" id="" class="campo" bind:value={proceso} on:change={elegirProceso} on:blur="">
    <option value="deposito">Depósito</option>
    <option value="retiro">Retiro</option>
    <option value="transferencia">Transferencia</option>
</select>

{#if proceso == 'deposito'}
    <Deposito on:ejecutar={ejecutar} {proceso}/>
{:else if proceso == 'retiro'}
    <Retiro on:ejecutar={ejecutar} {proceso}/>
{:else}
    <Transferencia on:ejecutar={ejecutar} {proceso}/>
{/if}