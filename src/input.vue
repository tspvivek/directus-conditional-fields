<template>

</template>

<script>
  import mixin from "@directus/extension-toolkit/mixins/interface";

  export default {
    mixins: [mixin],
    mounted() {
	
	  //Hide all conditional-fields-list with field names starting as conditional_
	  const fieldsNode = document.querySelectorAll('[data-field]');
	  this.hideAllConditionalInterface(fieldsNode);
	  

      const { values } = this._props
	  const conditions = this._props.options.conditions;	  
	  
	  
	  let populatedConditions = {};

	  	//Get list of conditional fields to check.
        Object.keys(conditions).forEach(( key, index ) => {

			const typeField = document.querySelector('[data-field='+key+']');			
			let conditional_fields=[];			
			let conditional_values={};
			
			// Get list of values to check for.
			Object.keys(conditions[key]).forEach(( keyValues, indexValues ) => {	
				
				let show_fields=[];				
				
				//Get list of fields to show.
				for (let i = 0; i < conditions[key][keyValues].length; i++) {
					conditional_fields.push(conditions[key][keyValues][i]);
					show_fields.push(conditions[key][keyValues][i]);
				}
				conditional_values[keyValues]=show_fields;
			});
			
			// Create a newly populated conditions with "all" for hiding fields and "show" for showing fields
			populatedConditions[key]={"all":conditional_fields, "show": conditional_values};			
			
			if (typeField) {
			
				  // Show or hide fields if value is already set.
				  if(populatedConditions[key].all)
				  {
					this.hideAll(populatedConditions[key].all);
				  }
				  if(populatedConditions[key].show[values[key]])
				  {
					this.showAll(populatedConditions[key].show[values[key]]);
				  }				
			    
				// Add event listener for 'change' event to hide and show fields.
				typeField.addEventListener('change', (e) => {
				  const value = e.target.value
				  const field = e.currentTarget.attributes['data-field'].value;
				  
				  if(populatedConditions[field].all)
				  {
					this.hideAll(populatedConditions[field].all);
				  }
				  if(populatedConditions[field].show[value])
				  {
					this.showAll(populatedConditions[field].show[value]);
				  }
				})
			} 
        });	
    },
    methods: {
      emitValue(event) {
        const value = event.target.value;
        this.$emit("input", value);
      },
      showAll(fieldsNode) {
		//show all fields
        for (let i = 0; i < fieldsNode.length; i++) {		
		  const field = document.querySelector('[data-field='+fieldsNode[i]+']');	
		  if(field)		  
		  {
		  field.style.display = 'block';
		  }
        }
      },
      hideAll(fieldsNode) {
	  	//hide all fields
        for (let i = 0; i < fieldsNode.length; i++) {		
		  const field = document.querySelector('[data-field='+fieldsNode[i]+']');
		  if(field)
		  {
			field.style.display = 'none';
		  }
        }
      },
	  
      hideAllConditionalInterface(fieldsNode) {
	  	//Hide all field with name starting with conditional_
        for (let i = 0; i < fieldsNode.length; i++) {
          let field = fieldsNode[i].dataset.field
          if (field.startsWith('conditional_'))
            fieldsNode[i].style.display = 'none'
        }
      }	  
	  
    }
  }
</script>

<style lang="scss" scoped>
.interface-json {
	position: relative;
	::v-deep {
		.CodeMirror-scroll {
			min-height: var(--form-column-width);
			max-height: var(--form-row-max-height);
		}
	}
}
button {
	position: absolute;
	top: 10px;
	right: 10px;
	user-select: none;
	color: var(--blue-grey-300);
	cursor: pointer;
	transition: color var(--fast) var(--transition-out);
	z-index: 10;
	&:hover {
		transition: none;
		color: var(--blue-grey-600);
	}
}
</style>
