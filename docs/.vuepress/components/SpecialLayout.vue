<template>
  <div>
    test
    <drop-down class="my-dropdown-toggle"
        :options="arrayOfObjects" 
        :selected="object" 
        v-on:updateOption="methodToRunOnSelect" 
        :placeholder="'Select an Item'"
        :closeOnOutsideClick="boolean">
    </drop-down>
    <Content />
  </div>

</template>

<script type="text/babel">
  import dropDown from './Dropdown.vue'
  export default {
    data() {
      return {
        type: false,
        arrayOfObjects: [
          {
            name:'glen'
          },
          {
            name:'selina'
          },
          {
            name: 'stanley'
          }
        ],
        object: {
          name: 'Object Name',
        },
        boolean: true
      };
    },
    components: {
      dropDown
    },
    computed: {
      typeClass() {
        return this.type
      },
    },
    methods: {
      handleAfterLeave() {
        this.$destroy(true);
        this.$el.parentNode.removeChild(this.$el);
      },
      methodToRunOnSelect(payload) {
        this.object = payload;
        console.log(payload);
      }
    }
  };
</script>
<style >
/* .my-dropdown-toggle {
  border-radius: 5px;

} 

.my-dropdown-toggle .dropdown-toggle {
  font-size: 25px;
  font-weight: 800;
}

.my-dropdown-toggle .dropdown-toggle-placeholder {
  color: #c4c4c4;
} */
</style>
