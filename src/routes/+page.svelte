<script>
	import { writable } from 'svelte/store';
  	import { Table, TableBody, TableBodyCell, TableBodyRow, TableHead, TableHeadCell } from 'flowbite-svelte';
	import { Button } from 'flowbite-svelte';
	const tableData = writable([]);

	tableData.subscribe(data => console.log("data changed. currentData length is", data.length))

	tableData.set([
		{ id: 1, maker: 'Toyota', type: 'ABC', make: 2017 },
		{ id: 2, maker: 'Ford', type: 'CDE', make: 2018 },
		{ id: 3, maker: 'Volvo', type: 'FGH', make: 2019 },
		{ id: 4, maker: 'Saab', type: 'IJK', make: 2020 }
	])


	const handleDelete = async (removeId) => {
		tableData.update(currentData => currentData.filter(item => item.id !== removeId));
		alert(`deleted: ${removeId}`);
    };
</script>

<Table hoverable={true} items={$tableData}>
  <TableHead>
    <TableHeadCell sort={(a, b) => a.id - b.id}>ID</TableHeadCell>
    <TableHeadCell sort={(a, b) => a.maker.localeCompare(b.maker)} defaultSort>Maker</TableHeadCell>
    <TableHeadCell sort={(a, b) => a.type.localeCompare(b.type)}>Type</TableHeadCell>
    <TableHeadCell sort={(a, b) => a.make - b.make} defaultDirection="desc">Make</TableHeadCell>
    <TableHeadCell>
      <span class="sr-only">Buy</span>
    </TableHeadCell>
  </TableHead>
  <TableBody tableBodyClass="divide-y">
    <TableBodyRow slot="row" let:item>
      <TableBodyCell>{item.id}</TableBodyCell>
      <TableBodyCell>{item.maker}</TableBodyCell>
      <TableBodyCell>{item.type}</TableBodyCell>
      <TableBodyCell>{item.make}</TableBodyCell>
      <TableBodyCell>
		<Button on:click={()=>handleDelete(item.id)} color="red">Delete</Button>
      </TableBodyCell>
    </TableBodyRow>
  </TableBody>
</Table>