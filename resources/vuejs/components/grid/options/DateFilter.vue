<template>
	<li class="date-filter from-date">
		<label :for="fromid" class="vuejsplus-data-grid-option-date-filter">{{ labelfrom }}</label>
		<input
			:id="fromid"
			type="date"
			name="fromdate"
			@change="doEmit"></input>
	</li>
	<li class="date-filter to-date">
		<label :for="toid" class="vuejsplus-data-grid-option-date-filter">{{ labelto }}</label>
		<input
			:id="toid"
			type="date"
			name="todate"
			@change="doEmit"></input>
	</li>
</template>

<script>

// @vue/component
module.exports = exports = {
	name: 'DateFilter',
	components: {
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
		this.option.filter = {};
		this.option.dataIndex = this.dataIndex;
		this.option.type = this.dataType;

		this.option.filter.from = 0;
		this.option.filter.to = 0;
		return {
			label: this.option.label,
			labelfrom: mw.message( 'vuejsplus-data-grid-option-filter-date-from-label' ).toString(),
			labelto: mw.message( 'vuejsplus-data-grid-option-filter-date-to-label' ).toString(),
			fromid: 'vuejsplus-data-grid-option-filter-date-from-' + this.dataIndex,
			toid: 'vuejsplus-data-grid-option-filter-date-to-' + this.dataIndex
		};
	},
	methods: {
		doEmit: function ( event ) {
			event.preventDefault();
			event.stopPropagation();

			if ( $( event.target ).attr( 'name' ) === 'fromdate' ) {
				this.option.filter.from = $( event.target )[ 0 ].valueAsNumber;
			}

			if ( $( event.target ).attr( 'name' ) === 'todate' ) {
				this.option.filter.to = $( event.target )[ 0 ].valueAsNumber;
			}

			// For some reason
			this.option.callback = 'DateFilter';
			this.$emit( 'optionfilter', this.option );
  		}
	}
};

</script>

<style lang="css">
.vuejsplus-data-grid-option-date-filter {
	display: block;
	font-weight: normal;
	text-align: left;
}
</style>
