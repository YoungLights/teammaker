<script setup>
	import { v4 as uuidv4 } from 'uuid'
	import { ref } from 'vue'

	const test = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
	const cats = ["cat-1.svg","cat-2.svg","cat-3.svg","cat-4.svg","cat-5.svg","cat-6.svg","cat-7.svg","cat-8.svg"]

	const members = ref([
		{id: 1, name: "Sascha", img: "cat-2.svg"},
		{id: 2, name: "Marcel", img: "cat-3.svg"},
		{id: 3, name: "Andre", img: "cat-8.svg"},
		{id: 4, name: "Gino", img: "cat-7.svg"},
		{id: 5, name: "Dennis", img: "cat-5.svg"}
	])

	const memberName = ref()
	const overlay = ref(false)
	const teams = ref([])

	const generateTeams = () => {
		teams.value = []
		const shuffleMembers = [...members.value].sort(() => Math.random() - 0.5)
		console.log(shuffleMembers)
		for (let i = 0; i < shuffleMembers.length; i += 2) {
			const chunk = shuffleMembers.slice(i, i + 2)
			teams.value.push(chunk)
		}
		console.log(teams.value)
	}

	const deleteMember = (id) => {members.value = members.value.filter((member) => member.id !== id)}
	const addMember = () => {
		const random = Math.floor(Math.random() * cats.length)
		members.value.push({id: uuidv4() , name: memberName.value, img: cats[random]})
		memberName.value = ''
		overlay.value = false
	}
	const toggleOverlay = () => {
		overlay.value = !overlay.value
	}
</script>

<template>
	<main class="maker">
		<div class="maker-header">
			<h1>Team Maker</h1>
			<span @click="toggleOverlay">+</span>
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
			<button class="maker-button" @click="generateTeams">generate teams</button>
		</div>
	</main>

	<div class="maker-overlay" :class="{active: overlay}">
		<div>
			<h4>Enter Member Name</h4>
			<input type="text" v-model="memberName">
			<button class="maker-button" @click="addMember">save</button>
			<span @click="toggleOverlay"><i class="fa-solid fa-xmark"></i></span>
		</div>
	</div>

	<div v-if="teams.length > 0" class="maker-teams">
		<div class="team" v-for="team in teams">
			<div v-for="member in team">
				{{ member.name }}
			</div>
		</div>
	</div>
</template>