<script>
	import {onMount} from 'svelte';
	import Listado from './components/Listar.svelte';
	import Formulario from './components/Formulario.svelte';
	//let urlBase = "http://restapi.test/"
	let urlBase = "https://bancoapi-oquendo.herokuapp.com/"
	
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

	const enviar = () => {
		traerCuentas();
	}

    onMount(()=>{
        traerCuentas();
    })
</script>

<main>
	<h1>Procesos bancarios</h1>

	<div class="container">
		<header>
			<div class="contenido-principal contenido">
				<div class="row">
					<div class="one-half column">
						<Formulario on:enviar={enviar}/>
					</div>
					<div class="one-half column">
						<Listado {cuentas}/>
					</div>
				</div>
			</div>
		</header>
	</div>

</main>

<style>

</style>