<template>
  <el-input readonly :value="optionsValue" />
</template>

<script>
const defaultProps = {
  value: "value",
  label: "label",
  split: "/"
};

export default {
  props: {
    value: {
      type: [String, Number, Array],
      required: false,
      default: ""
    },
    options: {
      type: Array,
      required: false,
      default: function() {
        return [];
      }
    },
    props: {
      type: Object,
      required: false,
      default: function() {
        return {};
      }
    }
  },
  data() {
    return {
     
    };
  },
  computed: {
    propsData() {
      return Object.assign({}, defaultProps, this.props || {});
    },
    optionsData() {
      const trans = array => {
        if (Array.isArray(array)) {
          return array.map(e => {
            return {
              value: e[this.propsData.value] || "",
              label: e[this.propsData.label] || "",
              children: trans(e.children)
            };
          });
        } else {
          return array;
        }
      };

      return trans(this.options);
    },
    optionsValue() {
      let value = this.value;
      let __ = '';
      if (Array.isArray(value)) {
        let result = [];
        let _catch = this.optionsData;
        for (let i = 0; i < value.length; i++) {
          const v = value[i];
          const targetIndex = _catch.findIndex(e => {
            return e.value === v;
          });
          //console.log(_catch, targetIndex)
          if (targetIndex !== -1) {
            result.push(_catch[targetIndex].label);
            if (Array.isArray(_catch[targetIndex].children)) {
              _catch = _catch[targetIndex].children;
            } else {
              break;
            }
          } else {
            break;
          }
        }
        __ = result.join(this.propsData.split);
      } else if (value !== void 0) {
        const targetIndex = this.optionsData.findIndex(e => {
          return e.value === value;
        });
        if (targetIndex !== -1) {
          __ = this.optionsData[targetIndex].label;
        } else {
          __ = "";
        }
      }
      return __
    }
  }
  
};
</script>

<style scoped>
</style>
