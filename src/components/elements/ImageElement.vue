<template>
	<div :id="settings.id" :data-testid="settings.id" @click="$emit('clickedOnElement')" @finished-editing-element="$emit('finished-editing-element')" :class="locals.classType + ' element'" :style="settings.styles" ref="element">
		<img class="image" draggable="false" :src="settings.configs.imageSrc" alt="Image" />
		<Resizers :query="`imageelement-${settings.id}`" />
	</div>
</template>

<script>
	export default {
		name: "ImageElement",
		props: {
			options: Object,
		},
		emits:['clickedOnElement', 'finished-editing-element'],
		mounted() {
			if (this.settings.grandParent === "TemplateBuilder") { // Initialize on moutned if its the template builder mode
				this.Initialize(this.$refs.element, `${this.locals.classType}-${this.settings.id}`, this.settings)
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
					classType: "imageelement",
				},
				settings: {
					grandParent: 'TemplateBuilder',
					id: 0,
					configs: {
						imageSrc: '',
					},
					styles: {},
				},
			}
		},
	};
</script>

<style>
</style>