<template>
  <div
    ref="content"
    class="hz-m-wrap"
  >
    <img class="hz-u-img" :src="image" />
    <ul
      class="hz-m-area"
      v-add-item
    >
      <zone
        class="hz-m-item"
        v-for="(zone, index) in zones"
        :key="index"
        :index="index"
        :name="zone.name"
        :setting="zone"
        :showDelete="false"
        @delItem="removeItem($event)"
        @changeInfo="changeInfo($event)"
      ></zone>
    </ul>
  </div>
</template>

<script>
import Zone from './Zone'
import addItem from '../directives/addItem'

export default {
  name: 'HotZone',
  props: {
    image: {
      type: String,
      required: true
    },
    zonesInit: {
      type: Array,
      default: () => []
    },
    max: {
      type: Number
    }
  },
  computed: {
    zones: function(){
      return this.zonesInit.concat()
      
      } 
  },
  methods: {
    changeInfo (res) {
      let { info, index } = res

      this.changeItem(info, index)
    },
    addItem (setting) {
      this.zones.forEach(element => {
        if(!element.name){
          this.removeItem(element.index);
        }
      });
      this.zones.push(setting)
      this.hasChange()
      this.$emit('add', setting)
    },
    eraseItem (index = this.zones.length - 1) {
      this.removeItem(index)
      this.$emit('erase', index)
    },
    isOverRange () {
      let { max, zones } = this

      return max && zones.length > max
    },
    overRange () {
      const index = this.zones.length - 1

      this.removeItem(index)
      this.$emit('overRange', index)
    },
    removeItem (index = this.zones.length - 1) {
      this.zones.splice(index, 1)
      this.hasChange()
      this.$emit('remove', index)
    },
    changeItem (info, index = this.zones.length - 1) {
      Object.assign(this.zones[index], info)
      this.hasChange(index)
    },
    hasChange (index = null) {
      this.$emit('change', this.zones, index)
    }
  },
  directives: {
    addItem
  },
  components: {
    Zone
  }
}
</script>

<style scoped>
@import '../assets/styles/main.css';
</style>
