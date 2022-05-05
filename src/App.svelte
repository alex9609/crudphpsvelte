<script>
	//Declarar un arreglo
	let empleados = [];
	//Este es como nuestro contenedor para la data de cada empleado
	let activado=true
	let activadoAgregar=false
	let datosEmpleado={
		id:null,
		nombre:'',
		correo:''
	}

	//Consultar o que es la información con un fetch
	//En este caso es la url donde vamos a solicitar los datos

	//Funciones que se ejecute costantenmente
	let mostrarEmpleados = () =>{
		//Pedir la respuesta en formato json
		activadoAgregar=false
		activado=true;
		fetch('http://localhost/empleados-main/')
		.then(respuesta=>respuesta.json())
		.then((datosRespuesta) => { //Me devuelve los datos respuesta
			empleados = datosRespuesta; //Almaceno los datos en un arreglo
			datosEmpleado={  //Evitando que se llene la informacion en este caso del formulario
				id:null,
				nombre:"",
				correo:""
			}
			console.log(empleados); //Imprimir el array de empleados
		}).catch(console.log) //Impresion de los rerores si hay errores
	}

	//Metodo para agregar empleados
	let agregarEmpleado = () =>{
		//Como recolactar la información
		const nuevoEmpledado={
			id:datosEmpleado.id,
			nombre: datosEmpleado.nombre,
			correo: datosEmpleado.correo
		}

		//Para insertar la API requiere de un parametroq eu seria insertar igual a 1
		fetch('http://localhost/empleados-main/?insertar=1',{
			method:"POST",
			body:JSON.stringify(nuevoEmpledado)
		})
		.then(respuesta=>respuesta.json())
		.then((datosRespuesta) => { //Me devuelve los datos respuesta
				mostrarEmpleados();
				console.log(empleados); //Imprimir el array de empleados
		}).catch(console.log) //repuesta
	}

	let borrarEmpleado = id =>{
		fetch('http://localhost/empleados-main/?borrar='+id)
		.then(respuesta=>respuesta.json())
		.then((datosRespuesta) => { //Me devuelve los datos respuesta
			mostrarEmpleados();
			console.log(empleados); //Imprimir el array de empleados
		}).catch(console.log) //Impresion de los rerores si hay errores
	}

	let editarEmpleado = empleado =>{
		activado=false;
		activadoAgregar=true
		datosEmpleado = empleado;
	}

	let actualizarEmpleado = () =>{
		fetch('http://localhost/empleados-main/?actualizar='+datosEmpleado.id,{
			method:"POST",
			body:JSON.stringify(datosEmpleado)
		})
		.then(respuesta=>respuesta.json())
		.then((datosRespuesta) => { //Me devuelve los datos respuesta
			mostrarEmpleados();
		}).catch(console.log) //Impresion de los rerores si hay errores
	}

	//Ejecutar la funcion para que se muestre de manera constanto los empleados
	mostrarEmpleados();

</script>
<div class="container">
	<div class="row">
		<div class="col-md-5">
			<div class="card">
				<div class="card-header">
					Empleados
				</div>
				<div class="card-body">
					<form>
						<div class="mb-3">
						  <label for="" class="form-label">ID</label>
						  <input
						  readonly=true
							bind:value={datosEmpleado.id}
						   type="text" class="form-control" name="" id="" aria-describedby="helpId" placeholder="">
						</div>

						<div class="mb-3">
							<label for="" class="form-label">Nombre</label>
							<input
							bind:value={datosEmpleado.nombre}
							type="text" class="form-control" name="" id="" aria-describedby="helpId" placeholder="">							
						</div>

						  <div class="mb-3">
							<label for="" class="form-label">Correo</label>
							<input 
							bind:value={datosEmpleado.correo}
							type="email" class="form-control" name="" id="" aria-describedby="helpId" placeholder="">
						  </div>
						  <!--Que hace el prevent Default prevetn default que hace que se ejecute una función-->
						  <button type="button" class="btn btn-success" 
						  on:click|preventDefault={agregarEmpleado}
						  disabled={activadoAgregar}
						  >Agregar empleado</button>

						  <button type="button" class="btn btn-primary"
						  on:click|preventDefault={actualizarEmpleado}
						  disabled={activado}
						  >Actualizar</button>

						  <button type="button" class="btn btn-danger"
						  on:click|preventDefault={mostrarEmpleados}
						  >Cancelar</button>
					</form>
				</div>
			</div>
		</div>
		<div class="col-md-7">
			<table class="table">
				<thead>
					<tr>
						<th>ID</th>
						<th>Nombre</th>
						<th>Correo</th>
						<th>Acciones</th>
					</tr>
				</thead>
				<tbody>
					{#each empleados as empleado}
					<tr>
						{#if empleado.id != undefined}
							<td>{empleado.id}</td>
							<td>{empleado.nombre}</td>
							<td>{empleado.correo}</td>

							<td>
								<button type="submit"
								class="btn btn-primary"
								on:click={editarEmpleado(empleado)}
								>Editar</button>
								<button type="submit"
								class="btn btn-danger"
								on:click={borrarEmpleado(empleado.id)}
								>Borrar</button>
							</td>
						{/if}
					</tr>
					{/each}
				</tbody>
			</table>
		</div>
		
	</div>
</div>


<style>
	
</style>