<template>
    <div class="btn-group">
        <li @click="toggleMenu()" class="dropdown-toggle" v-if="selectedOption.name !== undefined">
          {{ selectedOption.name }}
          <span :class="['arrow-down', showMenu? 'slide-down': '']"></span>
        </li>

        <li @click="toggleMenu()" class="dropdown-toggle dropdown-toggle-placeholder" v-if="selectedOption.name === undefined">
          {{placeholderText}}
          <span :class="['caret', showMenu? 'slide-down': '']"></span>
        </li>
        <div class="dropdown-menu" v-if="showMenu">
          <div class="dropdown-menu-item" v-for="(option, idx) in options" :key="idx">
              <a href="javascript:void(0)" @click="updateOption(option)">
                  {{ option.name }}
              </a>
          </div>
          <div class="dropdown-menu__arrow"></div>
        </div>
        
    </div>
</template>

<script>
    export default {
        data() {
            return {
                selectedOption: {
                  name: '',
                },
                showMenu: false,
                placeholderText: 'Please select an item',
            }
        },
        props: {
            options: {
                type: [Array, Object]
            },
            selected: {},
            placeholder: [String],
            closeOnOutsideClick: {
              type: [Boolean],
              default: true,
            },
        },
        mounted() {
            this.selectedOption = this.selected;
            if (this.placeholder)
            {
                this.placeholderText = this.placeholder;
            }
            if (this.closeOnOutsideClick) {
              document.addEventListener('click', this.clickHandler);
            }
        },
        beforeDestroy() {
            document.removeEventListener('click', this.clickHandler);
        },
        methods: {
            updateOption(option) {
                this.selectedOption = option;
                this.showMenu = false;
                this.$emit('updateOption', this.selectedOption);
            },
            toggleMenu() {
              this.showMenu = !this.showMenu;
            },
            clickHandler(event) {
                const { target } = event;
                const { $el } = this;
                if (!$el.contains(target)) {
                  this.showMenu = false;
                }
            },
        }
    }
</script>

<style>
.btn-group {
  min-width: 110px;
  height: 40px;
  position: relative;
  margin: 10px 1px;
  display: inline-block;
  vertical-align: middle;
}
.btn-group a:hover {
  text-decoration: none;
}
.dropdown-toggle {
  color: #636b6f;
  padding: 10px 20px 10px 10px;
  text-transform: none;
  font-weight: 300;
  border: 0;
  white-space: nowrap;
  text-overflow: ellipsis;
  font-size: 12px;
  /* overflow: hidden; */
}
.dropdown-toggle:hover {
  cursor: pointer;
}
.dropdown-menu {
  position: absolute;
  top: 100%;
  left: 0;
  z-index: 1000;
  min-width: 80px;
  padding: 5px 0;
  font-size: 14px;
  text-align: left;
  background-color: #fff;
  border: 1px solid #ccc;
  border-radius: 4px;
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.175);
}
.dropdown-menu > .dropdown-menu-item > a {
  padding: 5px 5px;
  display: block;
  clear: both;
  font-weight: normal;
  /* line-height: 1.6; */
  color: #333333;
  white-space: nowrap;
  text-decoration: none;
}
.dropdown-menu > .dropdown-menu-item > a:hover {
  background: #efefef;
  color: #409FCB;
}
.dropdown-menu > .dropdown-menu-item {
  overflow: hidden;
  width: 100%;
  position: relative;
  margin: 0;
}

.dropdown-menu__arrow,
.dropdown-menu__arrow::after {
    position: absolute;
    display: block;
    width: 0;
    height: 0;
    border-color: transparent;
    border-style: solid;
}

.dropdown-menu__arrow {
    top: -6px;
    left: 20%;
    margin-right: 3px;
    border-width: 6px;
    filter: drop-shadow(0 2px 12px rgba(0,0,0,.03));
    border-top-width: 0;
    border-bottom-color: #ccc;
}
.dropdown-menu__arrow::after {
    content: '';
    top: -4px;
    border-width: 5px;
    border-bottom-color: #fff;
    left: -5px;
}



.arrow-down {
  /* width: 0; */
  position: absolute;
  top: 10px;
  width: 20px;
  height: 20px;
  background: url('../public/arrow.png') no-repeat;
  background-size: contain;
  vertical-align: middle;
  /* border-top: 4px dashed;
  border-top: 4px solid \9;
  border-right: 4px solid transparent;
  border-left: 4px solid transparent; */
  right: 10px;
  transition: all 0.3s;
}
.arrow-down.slide-down {
  transform: rotate(180deg);
}


li {
    list-style: none;
}
</style>