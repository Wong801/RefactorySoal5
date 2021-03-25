<template>
  <div class="header flex justify-between py-8 px-8 text-white">
    <h1 class="text-3xl font-bold">Logo</h1>
		<div class="profile flex">
			<img :src="users.photo" alt="profile image">
			<h1 class="text-xl my-2 font-bold mx-5">{{ users.name }}</h1>
			<a href="#"><span class="fa fa-cog my-2 text-xl ml-2"></span></a>
		</div>
  </div>
</template>

<script>
export default {
	name: 'Header',
	props: ['users']
}
</script>

<style scoped>
.header{
	background-color: #0442D0;
}
.profile img {
	max-width: 52px;
	max-height: 52px;
	border-radius: 100px;
}
</style>