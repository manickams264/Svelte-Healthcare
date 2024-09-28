<script>
  let services = [
    { name: "General Checkup", description: "Routine health checkup", price: 50 },
    { name: "Blood Test", description: "Complete blood count test", price: 30 },
  ];

  let name = "";
  let description = "";
  let price = "";
  let editingIndex = null;

  const addOrUpdateService = () => {
    if (!name || !description || !price) return;

    const newService = { name, description, price };

    if (editingIndex === null) {
      services = [...services, newService];
    } else {
      services[editingIndex] = newService;
      editingIndex = null;
    }

    clearForm();
  };

  const deleteService = (index) => {
    services = services.filter((_, i) => i !== index);
  };

  const editService = (index) => {
    const service = services[index];
    name = service.name;
    description = service.description;
    price = service.price;
    editingIndex = index;
  };

  const clearForm = () => {
    name = "";
    description = "";
    price = "";
    editingIndex = null;
  };
</script>

<div class="container">
  <h2 class="fade-in">Healthcare Services</h2>

  <!-- Form for adding/updating services -->
  <div class="form">
    <input
      type="text"
      bind:value={name}
      placeholder="Service Name"
      class="fade-in input-style"
    />
    <input
      type="text"
      bind:value={description}
      placeholder="Service Description"
      class="fade-in input-style"
    />
    <input
      type="number"
      bind:value={price}
      placeholder="Service Price"
      class="fade-in input-style"
    />
    <button on:click={addOrUpdateService} class="fade-in btn gradient-btn">
      {editingIndex === null ? 'Add Service' : 'Update Service'}
    </button>
    {editingIndex !== null && (
      <button on:click={clearForm} class="fade-in btn secondary-button gradient-btn-secondary">
        Cancel
      </button>
    )}
  </div>

  <!-- Service list -->
  {#each services as service, index (service.name)}
    <div class="service fade-in">
      <div class="service-content">
        <strong>{service.name}</strong>
        <p class="service-description">{service.description}</p>
        <p class="service-price">${service.price}</p>
      </div>
      <div>
        <button on:click={() => editService(index)} class="btn gradient-btn fade-in">Edit</button>
        <button on:click={() => deleteService(index)} class="secondary-button gradient-btn-secondary fade-in">Delete</button>
      </div>
    </div>
  {/each}
</div>

<style>
  .fade-in {
    opacity: 0;
    transform: translateY(20px);
    animation: fadeIn 0.8s ease forwards;
  }

  @keyframes fadeIn {
    from {
      opacity: 0;
      transform: translateY(20px);
    }
    to {
      opacity: 1;
      transform: translateY(0);
    }
  }
</style>
