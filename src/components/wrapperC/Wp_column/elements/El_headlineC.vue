<template>
  <div class="w-full flex flex-col items-center" v-on-clickaway="save" >
    <textarea cols="15" rows="3" type="text" v-if="show_input"
      class="w-full text-35px font-semibold" :value="data_value.value || headline_value" @input="headline_value = $event.target.value" ></textarea>
    <h1 class="text-35px font-semibold" v-else
      @click="updateShowInfo">{{ data_value.value.length === 0 ? 'Enter Headline here' : data_value.value }}</h1>
    <slot name="delete_icon"></slot>
  </div>
</template>

<script>
import { mapStateVModel } from 'map-state-vmodel'
import { mixin as clickaway } from 'vue-clickaway';

export default {
  name: 'headlineElementC',
  extends: {},
  mixins: [ clickaway ],
  props: {
    data_value: {
      type: Object,
      required: true
    },
    data_index: {
      type: Number,
      required: true
    },
    section_index: {
      type: Number,
      required: true
    },
    row_index: {
      type: Number,
      required: true
    },
    column_index: {
      type: Number,
      required: true
    }
  },
  data(){
    return{
      show_input: false,
      headline_value: ''
    };
  },
  filters: {},
  watch: {},
  computed: {
    ...mapStateVModel('rightModal', ['show_rightModal', 'button_clicked', 'option_clicked']),
    ...mapStateVModel('pageData', ['wpSections', 'clicked_section', 'clicked_row', 'clicked_column']),
  },
  methods: {
    updateShowInfo() {
      this.show_input = true
    },
    async save() {
      let element = await this.wpSections[this.section_index].row_list[this.row_index][this.column_index].element_list
      await element.splice(this.data_index, 1, {
        "name": "headline",
        "icon": "icon-add-section",
        "value": `${this.headline_value}`
      })
      this.show_input = await false
    },
  },
  components: {},
  created() {},
  beforeCreate(){},
  mounted(){}
}
</script>
