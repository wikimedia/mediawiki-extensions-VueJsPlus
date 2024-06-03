<template>
	<th
		tabindex="0"
		:class="thClass"
		:aria-label="ariaLabel"
		:aria-controls="menuId"
		aria-haspopup="true"
		aria-expanded="false"
		:aria-role="ariaRole"
		:aria-sorted="col.ariaSorted"
		:data-colIndex="dataIndex"
		@click="toggleOptionsList"
		@keyup.enter="toggleOptionsList"
		@keyup-space="toggleOptionsList"
	>
		<span>{{ label }}</span>
		<col-options
			v-if="hasOptions"
			:id="menuId"
			:data-index="dataIndex"
			:data-type="dataType"
			:options="options"
			@update:model-value="doUpdateModel"
			@toggle-options-menu="onOptionsFocusout"
		></col-options>
	</th>
</template>

<script>
const ColOptions = require( './ColOptions.vue' );

// @vue/component
module.exports = exports = {
	name: 'DataCol',
	components: {
		'col-options': ColOptions
	},
	props: {
		col: {
			type: Object
		}
	},
	emits: [
		'update:model-value'
	],
	data: function () {
		let hasOptions = false;
		if ( this.col.hasOwnProperty( 'options' ) && this.col.options.length > 0 ) {
			hasOptions = true;
		}
		if ( this.col.sortable === false ) {
			hasOptions = false;
		}
		if ( this.col.filterable === false ) {
			hasOptions = false;
		}
		if ( this.dataIndex === '' ) {
			hasOptions = false;
		}

		let ariaLabel = this.col.label;
		const ariaRole = [ 'columnheader' ];
		const thClass = [ 'opt-menu' ];
		if ( hasOptions ) {
			ariaLabel = mw.message( 'vuejsplus-data-grid-option-button-aria-label', this.col.label ).toString();
			ariaRole.push( 'button' );
			thClass.push( 'opt-menu-btn' );
		}

		return {
			dataIndex: this.col.dataIndex,
			dataType: this.col.type.toLowerCase(),
			label: this.col.label,
			options: this.col.options,
			menuId: 'opt-menu-' + this.col.dataIndex,
			hasOptions: hasOptions,
			thClass: thClass.join( ' ' ),
			ariaRole: ariaRole.join( ' ' ),
			ariaLabel: ariaLabel
		};
	},
	methods: {
		toggleOptionsList: function ( event ) {
			event.stopPropagation();
			let btn = $( event.target );
			if ( $( btn ).parents( '.opt-list' ).length > 0 ) {
				// click on option in options list
				return;
			}
			if ( $( btn ).hasClass( 'opt-menu' ) === false ) {
				btn = $( btn ).parents( '.opt-menu' ).first();
			}

			if ( $( btn ).hasClass( 'opt-menu-btn' ) === false ) {
				return;
			}

			if ( $( btn ).attr( 'aria-expanded' ) === 'true' ) {
				$( btn ).attr( 'aria-expanded', 'false' );
				$( btn ).find( '.opt-list' ).first().removeClass( 'show' );
			} else {
				// close all other menus
				$( '.opt-menu' ).attr( 'aria-expanded', 'false' );
				$( '.opt-list' ).removeClass( 'show' );
				// open new menu
				$( btn ).attr( 'aria-expanded', 'true' );
				$( btn ).find( '.opt-list' ).first().addClass( 'show' );
			}
		},
		doUpdateModel: function ( option ) {
     		this.$emit( 'update:model-value', option );
  		},
		onOptionsFocusout: function () {
			const btn = $( 'th[data-index' + this.col.dataIndex + ']' );
			if ( $( btn ).attr( 'aria-expanded' ) === 'true' ) {
				$( btn ).attr( 'aria-expanded', 'false' );
				$( btn ).find( '.opt-list' ).first().removeClass( 'show' );
			} else {
				// close all other menus
				$( '.opt-menu' ).attr( 'aria-expanded', 'false' );
				$( '.opt-list' ).removeClass( 'show' );
				// open new menu
				$( btn ).attr( 'aria-expanded', 'true' );
				$( btn ).find( '.opt-list' ).first().addClass( 'show' );
			}
		}
	}
};

$( document ).on( 'click', function ( event ) {
	$( '.opt-menu' ).attr( 'aria-expanded', 'false' );
	$( '.opt-list' ).removeClass( 'show' );
} );

</script>

<style lang="css">
</style>
