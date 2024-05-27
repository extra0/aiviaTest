<template>
	<v-container>
		<v-container>
			<v-card class="mx-auto" max-width="350">
				<v-card-title>Let's play</v-card-title>
				<hr>
				<v-card-text>
					<v-text-field
						v-for="(item, type) in fields"
						:key="type"
						v-model="fields[type].value"
						:rules="item.rules"
						:label="item.label"
						class="mb-3"
						hide-details="auto"
						variant="outlined"
						required
					/>
				</v-card-text>
			</v-card>
		</v-container>

		<div v-if="list.length > 0" class="bg-grey-darken-2 pa-3 mt-4">
			<div v-for="(item, rowIndex) in list" :key="rowIndex" class="d-flex ma-1">
				<div
					v-for="(col, colIndex) in item"
					:key="colIndex"
					class="cube bg-white ma-2"
					@mouseenter="changeBackground"
				/>
			</div>
		</div>
	</v-container>
</template>

<script lang="ts" setup>
	import { watch, reactive, ref } from 'vue';

	const list = ref([]);
	const fields = reactive({
		x: {
			label: 'Size X',
			value: '',
		},
		y: {
			label: 'Size Y',
			value: '',
		},
	});

	watch(fields, (data) => {
		const x = +data.x.value;
		const y = +data.y.value;
		list.value = x > 0 && y > 0 ? Array(x).fill('').map(() => Array(y).fill('')) : [];
	});

	/**
	 * Change background block
	 * !!!!! this place could have been made more beautiful, I didn’t have time ()
	 * @param {Object} e
	 */
	function changeBackground(e) {
		const blueBg = 'bg-blue';
		const whiteBg = 'bg-white';
		const item = e.target;
		const isChanged = item.classList.contains(blueBg);

		if (isChanged) {
			item.classList.add(whiteBg);
			item.classList.remove(blueBg);
		} else {
			item.classList.add(blueBg);
			item.classList.remove(whiteBg);
		}
	}
</script>

<style scoped>
	.cube {
		width: 36px;
		height: 36px;
		cursor: pointer;
		transition: background .3s ease;
	}
</style>
