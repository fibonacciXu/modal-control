<template>
  <div class="main">
    <p>grandson孙组件</p>
    <a-modal
      v-model="visible"
      title="Modal"
      ok-text="确认"
      cancel-text="取消"
      @ok="hideModal"
      name="modalC"
    >
      <p>要展示在首页的弹框C</p>
    </a-modal>
  </div>
</template>

<script>
/* eslint-disable no-console */
import modalMap from "./modalConfig";
import createModalControl from "./ModalControl";
import { api1 } from "../../api";

const modalControl = createModalControl("index");

export default {
  data() {
    return {
      visible: false,
    };
  },
  components: {},
  created() {
    const modalList = modalMap.index.modalList;
    this.initApi(api1, modalList[2]);
  },
  methods: {
    initApi(apiName, modalItem) {
      apiName(modalItem).then((res) => {
        console.log("孙组件-接口数据获取成功:", res);
        // 接口的返回值控制弹窗的展示与否，所以加入弹窗管理实例中
        modalControl.add(modalItem, {
          backShow: res.backShow,
          handler: () => {
            console.log("孙组件-弹窗展示：", modalItem);
            this.visible = true;
          },
        });
      });
    },
    hideModal() {
      this.visible = false;
    },
  },
};
</script>
