<template>
	<div :id="settings.id" :data-testid="settings.id" ref="element" @click="$emit('clickedOnElement')" @finished-editing-element="$emit('finished-editing-element')" :class="locals.classType + ' element content-wrapper'" :style="settings.styles">
		<span class="content">
			{{ computedCounter }}
		</span>
		<Resizers :query="`pagecounter-${settings.id}`" />
	</div>
</template>

<script>
	export default {
		name: "PageCounter",
		props: {
			options: Object,
		},
		emits:['clickedOnElement', 'finished-editing-element'],
		mounted() {
			if (this.settings.grandParent === "TemplateBuilder") {
				this.Initialize(this.$refs.element, `${this.locals.classType}-${this.settings.id}`, this.settings)
			}
		},
		computed: {
			computedCounter() {
				if (this.settings.grandParent === "TemplateBuilder") { // Initialize on moutned if its the template builder mode
					if (this.settings.configs.completeForm) {
						if (this.settings.configs.persianNumbers) {
							return this.toPersianDigits('صفحه ۱ از ۱')
						}
						return 'page 1 / 1'
					}
					if (this.settings.configs.persianNumbers) {
						return this.toPersianDigits(this.settings.configs.counter)
					}
				}
				return this.settings.configs.counter
			}
		},
		watch: {
			options: {
				immediate: true,
				deep: true,
				handler(val) {
					this.settings = this.merge(this.settings, val)
				},
			},
		},
		data() {
			return {
				locals: {
					classType: "pagecounter",
				},
				settings: {
					grandParent: 'TemplateBuilder',
					id: 0,
					configs: {
						counter: '1',
						persianNumbers: true,
						completeForm: true,
					},
					styles: {},
				},
			}
		},
	};
</script>

<style>
</style>