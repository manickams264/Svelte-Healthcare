<script>
  let services = [
    { name: "General Checkup", description: "Routine health checkup", price: 50 },
    { name: "Blood Test", description: "Complete blood count test", price: 30 },
  ];

  let name = "";
  let description = "";
  let price = "";
  let editingIndex = null;

  // Function to add or update services
  const addOrUpdateService = () => {
    // Validate if fields are empty
    if (!name.trim() || !description.trim() || !price.trim()) {
      window.alert("All fields are required!");
      return;
    }

    // Convert price to a number and validate if it's a valid number
    const parsedPrice = parseFloat(price);
    if (isNaN(parsedPrice) || parsedPrice <= 0) {
      window.alert("Price must be a positive number!");
      return;
    }

    const newService = { name, description, price: parsedPrice };

    // If editingIndex is null, add the service; otherwise, update the service
    if (editingIndex === null) {
      services = [...services, newService]; // Add service to array
    } else {
      services[editingIndex] = newService; // Update service in array
      editingIndex = null; // Reset editingIndex after updating
    }

    clearForm(); // Clear form inputs after add/update
  };

  // Function to delete a service
  const deleteService = (index) => {
    services = services.filter((_, i) => i !== index);
  };

  // Function to edit a service
  const editService = (index) => {
    const service = services[index];
    name = service.name;
    description = service.description;
    price = service.price.toString(); // Ensure price is converted to a string for binding
    editingIndex = index;
  };

  // Function to clear the form
  const clearForm = () => {
    name = "";
    description = "";
    price = "";
    editingIndex = null;
  };
</script>

<!-- Main container -->
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
      type="text"
      bind:value={price}
      placeholder="Service Price"
      class="fade-in input-style"
    />

    <!-- Button for adding or updating a service -->
    <button on:click={addOrUpdateService} class="fade-in btn ripple">
      {editingIndex === null ? 'Add Service' : 'Update Service'}
    </button>

    <!-- Cancel button appears when editing -->
    {#if editingIndex !== null}
      <button on:click={clearForm} class="fade-in btn secondary-button ripple">
        Cancel
      </button>
    {/if}
  </div>

  <!-- List of services -->
  {#each services as service, index (service.name)}
    <div class="service fade-in">
      <div class="service-content">
        <strong>{service.name}</strong>
        <p class="service-description">{service.description}</p>
        <p class="service-price">${service.price.toFixed(2)}</p>
      </div>
      <div>
        <button on:click={() => editService(index)} class="btn ripple fade-in">Edit</button>
        <button on:click={() => deleteService(index)} class="secondary-button btn ripple fade-in">Delete</button>
      </div>
    </div>
  {/each}
</div>

<style>
  .ripple {
    position: relative;
    overflow: hidden;
  }

  .ripple:after {
    content: '';
    display: block;
    position: absolute;
    width: 100%;
    height: 100%;
    top: 0;
    left: 0;
    pointer-events: none;
    background: rgba(255, 255, 255, 0.4);
    transform: scale(10);
    transition: transform 0.5s ease-out, opacity 0.5s ease-out;
    opacity: 0;
  }

  .ripple:active:after {
    transform: scale(0);
    opacity: 1;
    transition: 0s;
  }

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
