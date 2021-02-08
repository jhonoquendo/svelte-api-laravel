<script>
    import Cuenta from './Cuenta.svelte';
    export let cuentas;
</script>
<h2>Listado de cuentas</h2>
<ul>
    {#each cuentas as cuenta}
        <Cuenta {cuenta}/>
    {:else}
        <li>No hay datos</li>
    {/each}
</ul>
