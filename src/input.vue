<template>

</template>

<script>
  import mixin from "@directus/extension-toolkit/mixins/interface";

  export default {
    mixins: [mixin],
    mounted() {
	
	
	  const fieldsNode = document.querySelectorAll('[data-field]');
	  this.hideAllConditionalInterface(fieldsNode);
	  

      const { values } = this._props
	  const conditions = this._props.options.conditions;	  
	  
	  console.log(this._props);
	  
	  let populatedConditions = {};

        Object.keys(conditions).forEach(( key, index ) => {
			console.log("Conditional field: "+key);
			const typeField = document.querySelector('[data-field='+key+']');			
			let conditional_fields=[];			
			let conditional_values={};

			Object.keys(conditions[key]).forEach(( keyValues, indexValues ) => {	
			    console.log("Value: "+keyValues);
				
				let show_fields=[];				
				
				console.log("fields to show");
				for (let i = 0; i < conditions[key][keyValues].length; i++) {
					conditional_fields.push(conditions[key][keyValues][i]);
					show_fields.push(conditions[key][keyValues][i]);
					console.log(conditions[key][keyValues][i]);
				}
				conditional_values[keyValues]=show_fields;
			});
			
			populatedConditions[key]={"all":conditional_fields, "show": conditional_values};
			
			console.log("populated");
			console.log(populatedConditions);
			
			if (typeField) {
			
				  if(populatedConditions[key].all)
				  {
					this.hideAll(populatedConditions[key].all);
				  }
				  if(populatedConditions[key].show[values[key]])
				  {
					this.showAll(populatedConditions[key].show[values[key]]);
				  }				
			    
				typeField.addEventListener('change', (e) => {
				  console.log("changed");
				  const value = e.target.value
				  const field = e.currentTarget.attributes['data-field'].value;
				  
				  console.log(populatedConditions);
				  console.log(field);
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
		console.log("showAll");
        for (let i = 0; i < fieldsNode.length; i++) {		
		  const field = document.querySelector('[data-field='+fieldsNode[i]+']');	
		  if(field)		  
		  {
		  field.style.display = 'block';
		  }
        }
      },
      hideAll(fieldsNode) {
	  	console.log("hideAll");
        for (let i = 0; i < fieldsNode.length; i++) {		
		  const field = document.querySelector('[data-field='+fieldsNode[i]+']');
		  if(field)
		  {
			field.style.display = 'none';
		  }
        }
      },
	  
      hideAllConditionalInterface(fieldsNode) {
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
