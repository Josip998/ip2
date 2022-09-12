<template>
	<v-app>
		<v-app-bar app shrink-on-scroll>
			<div class="header">
				<v-toolbar-title class="menu-item">
					<router-link exact active-class="active" to="/">
						Aplikacija
					</router-link>
				</v-toolbar-title>
				<v-toolbar-title class="menu-item">
					<router-link exact active-class="active" to="/about">
						About
					</router-link></v-toolbar-title
				>
			</div>
		</v-app-bar>
		<router-view class="view"></router-view>
	</v-app>
</template>


<script>
	export default {
		name: "App",
	};
</script>

<style lang="scss" scoped>
	.header {
		display: flex;
		justify-content: space-around;
		width: 100%;
		height: 100%;

		.menu-item {
			color: rgb(140, 140, 140);
			* {
				text-decoration: none;
			}
		}

		.menu-item:hover {
			cursor: pointer;
			color: rgb(255, 0, 0);
		}
	}
	.view {
		margin-top: 150px;
		height: 100%;
	}

	.active {
		color: rgb(2, 78, 184);
		font-weight: 500;
	}
</style>