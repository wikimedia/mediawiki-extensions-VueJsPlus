<template>
	<tr
		v-show="isVisible"
		:class="rowClass"
		:data-rowindex="rowIndex">
		<td
			is="datacell"
			v-for="cell in cells"
			:cell="cell"
			:row-index="rowIndex"
			@rowselected="handleSelect"
		></td>
	</tr>
</template>

<script>
const DataCell = require( './DataCell.vue' );

// @vue/component
module.exports = exports = {
	name: 'DataRow',
	components: {
		datacell: DataCell
	},
	props: {
		row: {
			type: Array,
			default: []
		}
	},
	emits: [
	],
	data: function () {
		// TODO: Ensure object properties
		return {
			rowClass: this.row.class,
			isVisible: this.row.isVisible,
			cells: this.row.data,
			rowIndex: this.row.rowIndex
		};
	},
	methods: {
		handleSelect: function ( data ) {
			if ( this.rowIndex === data.rowIndex ) {
				this.row.selected = data.selected;

				if ( data.selected === true ) {
					this.rowClass += ' selected';
				} else {
					this.rowClass = this.row.class;
				}
			}
		}
	}
};

</script>

<style lang="css">
</style>
