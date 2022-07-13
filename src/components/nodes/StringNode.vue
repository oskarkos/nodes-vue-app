<template>
  <div ref="el">
    <nodeHeader title="String" />
    <p>String</p>
    <el-input
      v-model="stringInput"
      placeholder="Please input"
      @input="updateInput"
    />
  </div>
</template>

<script>
import {
  defineComponent,
  ref,
  getCurrentInstance,
  nextTick,
  onMounted,
} from "vue";
import nodeHeader from "./nodeHeader.vue";
import { ElMessageBox } from "element-plus";

export default defineComponent({
  components: {
    nodeHeader,
  },
  setup() {
    const el = ref(null);
    const textarea = ref("");
    let df = null;
    const nodeId = ref(0);
    const dataNode = ref({});
    const stringInput = ref("");
    df = getCurrentInstance().appContext.config.globalProperties.$df.value;

    const updateInput = (value) => {
      dataNode.value.data.string = value;
      df.updateNodeDataFromId(nodeId.value, dataNode.value);
      console.log(dataNode.value);
    };

    onMounted(async () => {
      await nextTick();
      nodeId.value = el.value.parentElement.parentElement.id.slice(5);
      console.log(nodeId.value);
      dataNode.value = df.getNodeFromId(nodeId.value);
      console.log(dataNode.value);
      stringInput.value = dataNode.value.data.string;
      console.log(stringInput.value);
    });

    return {
      el,
      textarea,
      stringInput,
      updateInput,
    };
  },
});
</script>
<style scoped>
p {
  margin: 5px;
  margin-bottom: 10px;
}
</style>
