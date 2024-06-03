<template>
	<li>
		<button
			:class="spec.class"
			@click="buttonClick"
		>
			{{ spec.label }}
		</button>
	</li>
</template>

<script>

// @vue/component
module.exports = exports = {
	name: 'Item',
	components: {
	},
	props: {
		spec: {
			type: Array,
			default: []
		}
	},
	emits: [
		'itemclick'
	],
	data: function () {
		return {
			spec: this.spec
		};
	},
	methods: {
		buttonClick: function () {
			this.$emit( 'itemclick', this.spec );
		}
	}
};

</script>

<style lang="css">
.vuejsplus-toolbar-items button {
	background-color: transparent;
	border: none;
	height: 3em;
	padding: 0 1em;
}
</style>
