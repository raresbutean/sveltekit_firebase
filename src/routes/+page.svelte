<script lang="ts">
	import { addDoc, collection, doc, getFirestore, onSnapshot, query } from 'firebase/firestore';
	import db from '$lib/firestore';
	import { onMount } from 'svelte';

	const addUser = async () => {
		try {
			const docRef = await addDoc(collection(db, 'users'), {
				first: 'Anda',
				last: 'Moldovan',
				born: 2000
			});
			console.log('Document written: ', docRef);
		} catch (e) {
			console.error('Error creating doc ', e);
		}
	};

	let users = [{first:"test",last:"test2",born:1999}]
	onMount(()=>{
	const q = query(collection(db, 'users'));
	const unsub = onSnapshot(q, (qSnapshot) => {
		qSnapshot.forEach((doc) => users = [doc.data(),...users]);
		console.log('Users:',users);
	});
	});
</script>

<svelte:head>
	<title>Home</title>
	<meta name="description" content="Svelte demo app" />
</svelte:head>

<section>
	<button on:click={addUser}>Add Anda 💕</button>
	{#each users as user}
		<div>
			<p>Name: {user.first + " " + user.last}, born {user.born}</p>
		</div>
	{/each}
</section>