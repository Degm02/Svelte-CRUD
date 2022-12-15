<script>

	let empleados=[];
	let activado=true;
	let datosEmpleado={
		id:null,
		nombre:"",
		correo:""
	}

	let mostrarEmpleados =()=>{

		fetch('http://localhost/empleados/')
		.then(respuesta=>respuesta.json())
		.then((datosRespuesta)=>{
		empleados=datosRespuesta;
		datosEmpleado={
			id:null,
			nombre:"",
			correo:""
		}
		activado=true;
		console.log(empleados);

	}).catch(console.log)


	}

	let agregarEmpleado =()=>{

		const nuevoEmpleado ={
			id:datosEmpleado.id,
			nombre:datosEmpleado.nombre,
			correo:datosEmpleado.correo
		}

		fetch('http://localhost/empleados/?insertar=1',{
		method: "POST",
		body:JSON.stringify(nuevoEmpleado)
		})
		.then(respuesta=>respuesta.json())
		.then((datosRespuesta)=>{
			console.log(datosRespuesta);
			mostrarEmpleados();
	}).catch(console.log)

	}

	let borrarEmpleado=id=>{
		fetch('http://localhost/empleados/?borrar='+id)
	.then(respuesta=>respuesta.json())
	.then((datosRespuesta)=>{
		mostrarEmpleados();
	}).catch(console.log)

	}

	let editarEmpleado=empleado=>{
		activado=false;
		datosEmpleado=empleado;

	}
	
	let actualizarEmpleado=empleado=>{
		
		fetch('http://localhost/empleados/?actualizar='+datosEmpleado.id,{
		method: "POST",
		body:JSON.stringify(datosEmpleado)
		})
		.then(respuesta=>respuesta.json())
		.then((datosRespuesta)=>{
			console.log(datosRespuesta);
			mostrarEmpleados();
	}).catch(console.log)


	}
	mostrarEmpleados();

	
</script>

<div class="container">
		<div class="row">
			<div class="col-md-6">

				<div class="card">
						<div class="card-header">
							Empleados
						</div>
						<div class="card-body">
							<form action="" method="post">
								<div class="mb-3">
								  <label for="" class="form-label">ID</label>
								  <input readonly bind:value={datosEmpleado.id} 
								  type="text" class="form-control" name="" id="" aria-describedby="helpId" placeholder="">
								</div>

								<div class="mb-3">
									<label for="" class="form-label">Nombre</label>
									<input bind:value={datosEmpleado.nombre} 
									type="text" class="form-control" name="" id="" aria-describedby="helpId" placeholder="">
								  </div>

								  <div class="mb-3">
									<label for="" class="form-label">Correo</label>
									<input bind:value={datosEmpleado.correo} 
									type="text" class="form-control" name="" id="" aria-describedby="helpId" placeholder="">
								  </div>

								  <button type="button" disabled={!activado} class="btn btn-primary"
								  on:click|preventDefault={agregarEmpleado}> 
								  Agregar Empleado </button>

								  <button type="button" disabled={activado} class="btn btn-primary"
								  on:click|preventDefault={actualizarEmpleado}> 
								  Actualizar </button>

								  <button type="button" disabled={activado} class="btn btn-primary"
								  on:click|preventDefault={mostrarEmpleados}> 
								  Cancelar </button>
							</form>
						</div>
					</div>
				
			</div>

			<br>
			<br>
			<div class="col-md-6">
				<div class="table-responsive">
					<table class="table">
						<thead>
							<tr>
								<th>#</th>
								<th>Nombre</th>
								<th>Correo</th>
								<th>Acciones</th>
							</tr>
						</thead>
						<tbody>
							
							{#each empleados as empleado}
							<tr>
								<td>{empleado.id}</td>
								<td>{empleado.nombre}</td>
								<td>{empleado.correo}</td>
								<td> 
									<button
									type="submit"
									class="btn btn-warning"
									on:click={editarEmpleado(empleado)}
									>
									 Editar </button>
									| 
									<button
									type="submit"
									class="btn btn-danger"
									on:click={borrarEmpleado(empleado.id)}
									>
									 Borrar </button> 
								</td>
							</tr>
							{/each}
						</tbody>
					</table>
				</div>
			</div>
		</div>
	</div>
