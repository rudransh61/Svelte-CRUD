<script>
	let items = [
	  { id: 1, name: 'Item 3', date: '2022-01-01' },
	  { id: 2, name: 'Item 2', date: '2022-02-01' },
	  { id: 3, name: 'Item 1', date: '2022-03-01' },
	  // Add more sample data if needed
	];
  
	let formData = { name: '', date: '' };
	let editId = null;
	let filterText = '';
	let sortKey = 'name';
  
	const handleAdd = () => {
	  if (editId !== null) {
		// Edit existing item
		items = items.map(item => (item.id === editId ? formData : item));
		editId = null;
	  } else {
		// Add new item
		items = [...items, { id: Date.now(), ...formData }];
	  }
  
	  // Clear form data
	  formData = { name: '', date: '' };
	};
  
	const handleEdit = (id) => {
	  // Set form data for editing
	  const itemToEdit = items.find(item => item.id === id);
	  if (itemToEdit) {
		formData = { ...itemToEdit };
		editId = id;
	  }
	};
  
	const handleDelete = (id) => {
	  // Delete item
	  items = items.filter(item => item.id !== id);
	};
  
	$: filteredItems = items.filter(item => item.name.toLowerCase().includes(filterText.toLowerCase()));
  
	$: sortedItems = [...filteredItems].sort((a, b) => {
	  if (sortKey === 'name') {
		return a.name.localeCompare(b.name);
	  } else if (sortKey === 'date') {
		return new Date(a.date) - new Date(b.date);
	  }
	  return 0;
	});
  </script>
  
  <h1>Svelte CRUD App</h1>
  
  <input bind:value={formData.name} placeholder="Name" />
  <input bind:value={formData.date} placeholder="Date" />
  
  <button on:click={handleAdd}>Save</button>
  
  <label>
	Filter:
	<input bind:value={filterText} placeholder="Type to filter" />
  </label>
  
  <label>
	Sort by:
	<select bind:value={sortKey}>
	  <option value="name">Name</option>
	  <option value="date">Date</option>
	</select>
  </label>
  
  <ul>
	{#each sortedItems as item (item.id)}
	  <li>
		{item.name} - {item.date}
		<button on:click={() => handleEdit(item.id)}>Edit</button>
		<button on:click={() => handleDelete(item.id)}>Delete</button>
	  </li>
	{/each}
  </ul>
  