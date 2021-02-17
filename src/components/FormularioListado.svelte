<script>
	import {onMount} from 'svelte';
    
    import Deposito from './Deposito.svelte';
    import Retiro from './Retiro.svelte';
    import Transferencia from './Transferencia.svelte';
    import Listar from './Listar.svelte';
    
    //let urlBase = "http://restapi.test/"
	let urlBase = "https://bancoapi-oquendo.herokuapp.com/"
    let proceso = 'deposito';
	
    let cuentas = [];

    const traerCuentas =  async () => {
        await fetch(`${urlBase}listar`,{
            method:'GET',
            headers:{
                'Content-Type': 'application/json'
            }
        })
        .then((res) => res.json())
            .then((res) => {
                cuentas = res;
            });
    }

    onMount(()=>{
        traerCuentas();
    })

    const ejecutar = () => {
        traerCuentas();
    }
</script>
<div class="row">
    <div class="one-half column">
        <h2>Realiza tus procesos</h2>
        <select name="" id="" class="campo" bind:value={proceso}>
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
    </div>
    <div class="one-half column">
        <Listar {cuentas}></Listar>
    </div>
</div>