<template>
	<v-container>
		<v-card class="mx-auto" max-width="350">
			<v-form ref="loginForm" validate-on="submit lazy" @submit.prevent="submitForm">
				<v-card-title>Let's login</v-card-title>
				<hr>
				<v-card-text>
					<v-text-field
						v-for="(item, type) in fields"
						:key="type"
						v-model="fields[type].value"
						:rules="item.rules"
						:label="item.label"
						class="mb-3"
						:type="type"
						hide-details="auto"
						variant="outlined"
						required
					/>
				</v-card-text>
				<v-card-actions class="px-4">
					<v-btn
						size="x-large"
						color="indigo-darken-3"
						variant="flat"
						text="Submit"
						type="submit"
						block
					/>
				</v-card-actions>
			</v-form>
		</v-card>
	</v-container>
</template>

<script lang="ts" setup>
	import { ref, reactive } from 'vue';
	import { useRouter } from 'vue-router';

	const router = useRouter()
	const loginForm = ref();
	const fields = reactive({
		email: {
			label: 'Email',
			rules: [
				value => {
					if (!value) {
						return 'Please enter email'
					}

					if (!(/^[a-z.-]+@[a-z.-]+\.[a-z]+$/i.test(value))) {
						return 'Must be a valid e-mail.'
					}
				},
			],
			value: '',
		},
		password: {
			label: 'Password',
			rules: [
				value => {
					if (!value) {
						return 'Please enter password'
					}

					if (value.length < 6) {
						return 'Password must contain at least 6 characters'
					}
				},
			],
			value: '',
		},
	});

	/**
	 * Submit form
	 */
	async function submitForm() {
		loginForm.value?.validate().then(({ valid: isValid }) => {
			if (!isValid) {
				return;
			}

			router.push({ path: '/game' })
		});
	}
</script>
