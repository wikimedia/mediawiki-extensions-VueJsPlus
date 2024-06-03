<template>
	<ul
		class="opt-list"
		@focusout="doEmitFocusOut"
		@keyup.esc="doEmitEsc">
		<component
			:is="option.name"
			v-for="( option, index ) in options"
			:key="index"
			:data-index="dataIndex"
			:data-type="dataType"
			:option="option"
			@optionclick="doEmitClick"
			@optionfilter="doEmitFilter"
		></component>
	</ul>
</template>

<script>

const SortASC = require( './options/SortASC.vue' );
const SortDESC = require( './options/SortDESC.vue' );
const StringFilter = require( './options/StringFilter.vue' );
const DateFilter = require( './options/DateFilter.vue' );

// @vue/component
module.exports = exports = {
	name: 'ColOptions',
	components: {
		SortASC: SortASC,
		SortDESC: SortDESC,
		StringFilter: StringFilter,
		DateFilter: DateFilter
	},
	props: {
		options: {
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
		'update:model-value',
		'toggle-options-menu'
	],
	data: function () {
		const localOptions = this.options;

		return {
			options: localOptions,
			dataIndex: this.dataIndex,
			dataType: this.dataType
		};
	},
	methods: {
		doEmitClick: function ( option ) {
 			this.$emit( 'update:model-value', option );
  		},
		doEmitFilter: function ( option ) {
   			this.$emit( 'update:model-value', option );
  		},
		doEmitFocusOut: function ( event ) {
			const relatedTarget = event.relatedTarget;
			if ( $( relatedTarget ).hasClass( 'opt-menu' ) ) {
				this.$emit( 'toggle-options-menu' );
				$( event.target ).parents( 'th.opt-menu' ).first().trigger( 'focus' );
			}
  		},
		doEmitEsc: function ( event ) {
			$( event.target ).parents( 'th.opt-menu' ).first().trigger( 'focus' );
			this.$emit( 'toggle-options-menu' );
  		}
	}
};

</script>

<style lang="css">
.opt-list {
	position: absolute;
	left: auto;
	right: 0;
	transform: translateX( 95% );
	width: max-content;
	background-color: white;
	border: 1px solid black;
	border-radius: 5px;
	padding: 10px !important;
	list-style: none;
	display: none;
}
.opt-list.show {
	display: block;
	z-index: 5000;
}
.opt-list > li {
	padding: 5px 0;
}
.opt-list > li > button {
	width: 100%;
	background-color: white;
	border: none;
	text-align: left;
}

</style>
