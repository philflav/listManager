<script>
	// @ts-nocheck
	import { db } from '../firebase.js';
	import {
		collection,
		onSnapshot,
		doc,
		updateDoc,
		deleteDoc,
		addDoc,
		where,
		query,
		setDoc
	} from 'firebase/firestore';

	import { user, isLoggedIn } from '../stores/stores';

	const displayName = $user.displayName || 'nobody';

	const colRef = collection(db, 'shopping');
	const q = query(colRef, where('userUID', '==', $user.uid || ''));

	const shopRef = collection(db, 'shops');
	const qshops = query(shopRef, where('userUID', '==', $user.uid || ''));

	const fbshopping = [];
	let shopping = [];
	const shopName = ['shop1', 'shop2', 'shop3'];

	let name1 = 'xx';
	let name2 = 'xxx';
	let name0 = 'x';

	const _func = onSnapshot(q, (querySnapshot) => {
		let fbshopping = [];
		let bgs = ['bg-green-400', 'bg-orange-400', 'bg-blue-300'];
		querySnapshot.forEach((doc) => {
			let todo = { ...doc.data(), id: doc.id, bg: bgs[doc.data().shop] };
			fbshopping = [todo, ...fbshopping];
		});
		shopping = fbshopping.sort((a, b) => a.shop - b.shop);
	});
	const _func1 = onSnapshot(qshops, (querySnapshot) => {
		querySnapshot.forEach((doc) => {
			shopName[doc.data().index] = { name: doc.data().name, docId: doc.id };
		});
		name0 = shopName[0].name;
		name1 = shopName[1].name;
		name2 = shopName[2].name;
		console.log(name0, name1, name2);
	});

	let newItem = '';

	const addItem = async (shop) => {
		if (newItem.slice(0, 1) == '#') {
			changeName(shop);
		} else if (newItem !== '') {
			// console.log(priority);
			const docRef = await addDoc(collection(db, 'shopping'), {
				task: newItem,
				isComplete: false,
				userUID: $user.uid,
				shop: shop || 0
			});
			newItem = '';
		}
	};

	const changeName = async (shop) => {
		console.log(newItem, shopName[shop].docId);
		if (shopName[shop].docId != undefined) {
			console.log('updating ', shopName[shop].docId);
			await updateDoc(doc(db, 'shops', shopName[shop].docId), {
				name: newItem.slice(1)
			});
		} else {
			console.log('writing new doc');
			await addDoc(collection(db, 'shops'), {
				name: newItem.slice(1),
				userUID: $user.uid,
				index: shop
			});
		}
		newItem = '';
	};

	const keyPressed = (event) => {
		if (event.key === 'Enter') {
			addItem();
		}
	};

	const markCompleted = async (item) => {
		await updateDoc(doc(db, 'shopping', item.id), {
			isComplete: !item.isComplete
		});
	};

	const deleteTodo = async (item) => {
		await deleteDoc(doc(db, 'shopping', item.id));
	};

	let placeholder = 'Add a shopping item or #shopname';
</script>

<h1 class="text-center my-8 uppercase">{displayName}'s Shopping List</h1>
<div class="container m-6 mr-6">
	{#if $isLoggedIn}
		<p class="text-base pb-5">
			Enter shopping list items in field below and then click on the appropriate shop. Prefix item
			with '#' to change the shop name.
		</p>
		<input
			class="text-lg4 w-96 text-center m-3 bg-slate-200"
			type="text"
			{placeholder}
			bind:value={newItem}
		/>
		<div class="flex flex-col">
			<button
				class="bg-green-500 hover:bg-green-700 text-base text-white px-4 rounded-full m-3 w-96 drop-shadow-lg "
				on:click={() => {
					addItem(0);
				}}
			>
				{name0}</button
			>
			<button
				class="bg-orange-500 hover:bg-orange-700 text-base text-white px-4 rounded-full m-3 w-96 drop-shadow-lg"
				on:click={() => {
					addItem(1);
				}}
			>
				{name1}</button
			>
			<button
				class="bg-blue-400 hover:bg-blue-700 text-base text-white px-4 rounded-full m-3 w-96 drop-shadow-lg"
				on:click={() => {
					addItem(2);
				}}
			>
				{name2}</button
			>
		</div>

		<div class="border-2 m-2  p-1">
			<ul>
				{#each shopping as td}
					<li class="text-left text-base" class:complete={td.isComplete}>
						<span
							><button
								title="Mark task as bought"
								class="bg-blue-500 hover:bg-blue-700 text-white m-1 px-1 rounded-full"
								on:click={() => markCompleted(td)}>✔</button
							></span
						>
						<span
							><button
								title="Delete item"
								class="bg-red-500 hover:bg-red-700 text-white m-1 px-1 rounded-full"
								on:click={() => deleteTodo(td)}>&#128465</button
							></span
						>
						<span class={td.bg}>{td.task}</span>
						>
					</li>
				{:else}
					<p>No shopping found</p>
				{/each}
			</ul>
		</div>
	{/if}
	{#if !$isLoggedIn}
		<div class="text-center"><h2>User not logged in</h2></div>
	{/if}
</div>

<svelte:window on:keypress={keyPressed} />

<style>
	.complete {
		text-decoration: line-through;
		text-decoration-color: rgb(137, 68, 81);
	}
</style>
