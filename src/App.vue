<script setup>
	import { ref } from 'vue'

	function sliceIntoChunks(arr, chunkSize) {
		const res = []
		for (let i = 0; i < arr.length; i += chunkSize) {
			const chunk = arr.slice(i, i + chunkSize)
			res.push(chunk)
		}
		return res
	}

	const deleteMember = (id) => {
		members.value = members.value.filter((member) => member.id !== id)
	}

	const test = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
	const members = ref([
		{id: 1, name: "Sascha", img: "cat-2.svg"},
		{id: 2, name: "Marcel", img: "cat-3.svg"},
		{id: 3, name: "Andre", img: "cat-8.svg"},
		{id: 4, name: "Gino", img: "cat-7.svg"},
		{id: 5, name: "Dennis", img: "cat-5.svg"}
	])
</script>

<template>
	<main class="maker">
		<div class="maker-header">
			<h1>Team Maker</h1>
			<span>+</span>
			<div>Add member</div>
		</div>
		<div class="maker-list">
			<div v-if="members.length > 0">
				<div class="maker-item" v-for="member in members" :key="member.id">
					<div class="maker-item-avatar">
						<span><img :src="`./cats/${member.img}`" :alt="member.name"></span>
						<h4>{{ member.name }}</h4>
					</div>
					<span class="maker-item-delete" @click="deleteMember(member.id)"><i class="fa-solid fa-xmark"></i></span>
				</div>
			</div>
			<p v-else>There are no members yet</p>
		</div>
		<div v-if="members">
			<button class="maker-generate">generate teams</button>
		</div>
	</main>

	<div class="maker-overlay">
		<input type="text">
		<button>Add Name</button>
	</div>
</template>