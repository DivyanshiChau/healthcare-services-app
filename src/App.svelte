<script>
	let services = [
		{ id: 1, name: "General Checkup", description: "Routine health checkup", price: 50 },
		{ id: 2, name: "Blood Test", description: "Complete blood test", price: 30 },
	];

	let newService = { name: "", description: "", price: "" };
	let isEditing = false;
	let editIndex = null;

	function addService() {
		if (newService.name && newService.description && newService.price) {
			services = [...services, { ...newService, id: services.length + 1 }];
			resetForm();
		}
	}

	function editService(index) {
		isEditing = true;
		editIndex = index;
		newService = { ...services[index] };
	}

	function updateService() {
		services[editIndex] = { ...newService };
		isEditing = false;
		resetForm();
	}

	function deleteService(index) {
		services = services.filter((_, i) => i !== index);
	}

	function resetForm() {
		newService = { name: "", description: "", price: "" };
		editIndex = null;
		isEditing = false;
	}
</script>

<main>
	<h1>Healthcare Services</h1>

	<!-- List of services -->
	<div class="card-deck">
		{#each services as service, index}
			<div class="card mb-4">
				<div class="card-body">
					<h5 class="card-title">{service.name}</h5>
					<p class="card-text">{service.description}</p>
					<p class="card-text"><strong>Price:</strong> ${service.price}</p>
					<div class="d-flex justify-content-between">
						<button class="btn btn-warning" on:click={() => editService(index)}>Edit</button>
						<button class="btn btn-danger" on:click={() => deleteService(index)}>Delete</button>
					</div>
				</div>
			</div>
		{/each}
	</div>

	<!-- Add/Edit form -->
	<form on:submit|preventDefault={isEditing ? updateService : addService} class="form-group">
		<input
			type="text"
			placeholder="Service Name"
			bind:value={newService.name}
			required
			class="form-control mb-2"
		/>
		<input
			type="text"
			placeholder="Description"
			bind:value={newService.description}
			required
			class="form-control mb-2"
		/>
		<input
			type="number"
			placeholder="Price"
			bind:value={newService.price}
			required
			class="form-control mb-2"
		/>
		<button type="submit" class="btn btn-success">{isEditing ? "Update" : "Add"} Service</button>
		
		{#if isEditing}
			<button type="button" class="btn btn-secondary" on:click={resetForm}>Cancel</button>
		{/if}
	</form>
</main>

<style>
	body {
		font-family: 'Arial', sans-serif;
		background-color: #f4f7fa;
		color: #333;
		margin: 0;
		padding: 0;
		display: flex;
		justify-content: center;
		align-items: center;
		min-height: 100vh;
	}
	
	main {
		background-color: #fff;
		padding: 20px;
		border-radius: 8px;
		box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
		width: 100%;
		max-width: 600px;
		margin: 50px auto;
		display: flex;
		flex-direction: column;
		justify-content: center;
	}
	
	h1 {
		text-align: center;
		color: #007bff;
		font-size: 2.3rem;
		margin-bottom: 20px;
	}

	.card {
		border-radius: 2px;
		transition: transform 0.2s;
		margin: 10px;
	}
	
	.card:hover {
		transform: scale(1.02);
	}

	.btn {
		padding: 10px 15px;
		margin: 5px 0;
		border-radius: 12px !important;
		border: 2px solid grey;
	}

	.btn-warning {
		background-color: #ffc107;
		color: #fff;
		border-radius: 2px;
	}

	.btn-danger {
		background-color: #dc3545;
		color: #fff;
		border-radius: 2px;
	}

	.btn-success {
		background-color: #28a745;
		color: #fff;
	}

	.btn-secondary {
		background-color: #6c757d;
		color: #fff;
	}

	@media (max-width: 600px) {
		main {
			width: 90%;
		}
	}
</style>
