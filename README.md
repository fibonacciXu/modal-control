## 动态配置弹窗显示

```
// modalConfig.js
export default {
  // 首页
  index: {
    // 弹框列表
    modalList: [{
      id: 1, // 弹框的id
      name: 'modalA',
      level: 100,
      // 弹框的优先级
      // 由前端控制弹框是否显示
      // 当我们一个活动过去了废弃一个弹框时候，可以不需要通过后端去更改
      frontShow: true
    }, {
      id: 2,
      name: 'modalB',
      level: 122,
      frontShow: true
    }, {
      id: 3,
      name: 'modalC',
      level: 70,
      frontShow: true
    }]
  }
}
```
### 发布订阅模式管理弹窗+单例模式实例化弹窗类

```
class ModalControl {
  constructor (type) {
    this.type = type
    this.modalFlatMap = {}
    this.modalList = getAllModalList(modalMap[this.type])
  }

  add (modalItem, infoObj) {
    this.modalFlatMap[modalItem.name] = {
      id: modalItem.id,
      level: modalItem.level,
      frontShow: modalItem.frontShow,
      backShow: infoObj.backShow,
      handler: infoObj.handler
    }
    this.preCheck()
  }

  preCheck () {
    if (this.modalList.length === Object.values(this.modalFlatMap).length) {
      this.notify()
    }
  }

  notify () {
    const highLevelModal = Object.values(this.modalFlatMap).filter(item => item.backShow && item.frontShow).reduce((t, c) => {
      return c.level > t.level ? c : t
    }, { level: -1 })
    highLevelModal.handler && highLevelModal.handler()
  }
}

const controlTypeMap = {}
// 获取单例
function createModalControl (type) {
  if (!controlTypeMap[type]) {
    controlTypeMap[type] = new ModalControl(type)
  }
  console.log('controlTypeMap[type]', controlTypeMap[type])
  return controlTypeMap[type]
}

export default createModalControl
```

```
yarn

yarn serve
```