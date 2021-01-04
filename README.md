## 多个弹窗展示

### 发布订阅模式控制多对一+单例模式实例化弹窗

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