<template>
	<li>
		<cdx-search-input
			:clearable="true"
			:placeholder="label"
			:aria-label="label"
			@input="doEmit"
		></cdx-search-input>
	</li>
</template>

<script>

const { CdxSearchInput } = require( '@wikimedia/codex' );

// @vue/component
module.exports = exports = {
	name: 'StringFilter',
	components: {
		CdxSearchInput: CdxSearchInput
	},
	props: {
		option: {
			type: Array,
			default: []
		},
		dataIndex: {
			type: String,
			default: ''
		},
		dataType: {
			type: String,
			default: ''
		}
	},
	emits: [
		'optionfilter'
	],
	data: function () {
		this.option.filter = '';
		this.option.dataIndex = this.dataIndex;
		this.option.type = this.dataType;
		return {
			label: this.option.label
		};
	},
	methods: {
		doEmit: function ( event ) {
			event.preventDefault();
			event.stopPropagation();

			if ( event.inputType === 'insertText' ) {
				this.option.filter = this.option.filter + event.data;
			}
			if ( event.inputType === 'deleteContentBackward' ) {
				if ( this.option.filter !== '' ) {
					this.option.filter = this.option.filter.slice( 0, Math.max( 0, this.option.filter.length - 1 ) );
				}
			}
			// For some reason
			this.$emit( 'optionfilter', this.option );
  		}
	}
};

</script>

<style lang="css">
</style>
