<template>
<div class="container">
  <span id="title">Challenge Backend Rappi</span>
  <form v-on:submit.prevent="sendRequest">
  <div class="row" id="myApp">
    <div class="col-lg-3 col-md-3 col-xs-3">
      <v-select :on-change="changedValue" :options="['Query','Update']"></v-select>
    </div>
    <slot name="item" v-for="row in rows">
    <div :class="row.property">
		  <input class="form-control" id="add-name"  :placeholder="row.placeholder" required/>
	  </div>
    </slot>
  </div>
  <br/>
  <div class="col-lg-offset-8" id="send">
		<button type="submit" class="btn btn-primary">Agregar</button>
	</div>
  </form>
  <div id="instructions">
  <strong>Instructions</strong>
  <p>This is a paragraph.</p>
  </div>
</div>  

</template>
<script>
import vSelect from "vue-select"
  export default {
    components: {vSelect},

    data() {
      return {
        rows: []
      }
    },
    methods: {
      changedValue: function(value) {
        if(value === 'Query')
        {
          this.rows= []
          this.rows.push({ placeholder: "x1", property: "col-lg-1"}, 
                         { placeholder: "y1", property: "col-lg-1"}, 
                         { placeholder: "z1", property: "col-lg-1"},
                         { placeholder: "x2", property: "col-lg-1"}, 
                         { placeholder: "y2", property: "col-lg-1"}, 
                         { placeholder: "z2", property: "col-lg-1"});
        }
        else if(value === 'Update')
        {
          this.rows= []
          this.rows.push(
            { placeholder: "x", property: "col-lg-2"}, 
            { placeholder: "y", property: "col-lg-2"}, 
            { placeholder: "z", property: "col-lg-2"},
            { placeholder: "w", property: "col-lg-2"});
        }
          
  },
  sendRequest(e){
      console.log(e)
  }
    }
  }
</script>