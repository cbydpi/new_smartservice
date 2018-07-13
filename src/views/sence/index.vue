<template>
  <div>
    <!--这是场景录入页面,-->

    <!--<rich-text @richTextContent='getRichTextData'></rich-text>-->
    <sence-modul ref='diag' v-bind:model-data='diagramData' v-on:model-changed='modelChanged' v-on:changed-selection='changedSelection'></sence-modul>
    <!--<button v-on:click='addNode'>Add Child to Gamma</button>
    <button v-on:click='modifyStuff'>Modify view model data without undo</button>
    <br/>Current Node:
    <input v-model.lazy='currentNodeText' v-bind:disabled='currentNode === null' />
    <br/>The saved GoJS Model:
    <textarea style='width:100%;height:250px'>{{ savedModelText }}</textarea>-->
  </div>
</template>

<script>
  // 这是富文本引入
  import RichText from '@/components/rich-text'
  import go from 'gojs'
  //  export default {
  //  components: {
  //    RichText
  //  },
  //  methods: {
  //    getRichTextData (data) {
  //    console.log(data)
  //    }
  //  }
  //  }
  import SenceModul from './sence-modul'
  export default {
    components: {
      SenceModul,
      RichText
    },
    data () {
      return {
        diagramData: { // passed to <diagram> as its modelData
          nodeDataArray: [{
            'id': 1636,
            'contentId': null,
            'senceNo': '43',
            'content': '奥法的产品优势有：1.从通用化到场景化。2.从标准化到个性化。3.智能巡检。4.从技术到数据应用。您想详细了解哪个呢？',
            'level': 0,
            'parentId': 0,
            'keyword': '(奥法).*(产品).*(优势)',
            'description': '奥法产品的优势是什么',
            'status': 0,
            'weight': null,
            'username': 'admin',
            'contentSign': 1610,
            'voiceId': null,
            'voice': null,
            'remark': null,
            'extendList': null,
            'commadList': [],
            'contentType': 1,
            'cbContent': ''
          }, {
            'id': 1637,
            'contentId': null,
            'senceNo': '43',
            'content': '通用化到场景化，既可以实现通用法问答服务，也拥有灵活的场景化服务，引导用户与机器人流畅的对话，多轮交互，提高体验。',
            'level': 1,
            'parentId': 1636,
            'keyword': '(通用化)',
            'description': '通用化',
            'status': 2,
            'weight': null,
            'username': 'admin',
            'contentSign': 1612,
            'voiceId': null,
            'voice': null,
            'remark': null,
            'extendList': null,
            'commadList': [],
            'contentType': 1,
            'cbContent': ''
          }, {
            'id': 1638,
            'contentId': null,
            'senceNo': '43',
            'content': '从标准化到个性化，是指，针对不同的业务领域，将配置不同的数据训练模式、积累不同自学习逻辑，达到精准服务。',
            'level': 1,
            'parentId': 1636,
            'keyword': '(标准化)',
            'description': '标准化',
            'status': 2,
            'weight': null,
            'username': 'admin',
            'contentSign': 1613,
            'voiceId': null,
            'voice': null,
            'remark': null,
            'extendList': null,
            'commadList': [],
            'contentType': 1,
            'cbContent': ''
          }, {
            'id': 1639,
            'contentId': null,
            'senceNo': '43',
            'content': '智能巡检是指，实时监控数据，准确把握数据积累过程，确保知识的准确性。',
            'level': 1,
            'parentId': 1636,
            'keyword': '(智能).*(巡检)',
            'description': '智能巡检',
            'status': 2,
            'weight': null,
            'username': 'admin',
            'contentSign': 1614,
            'voiceId': null,
            'voice': null,
            'remark': null,
            'extendList': null,
            'commadList': [],
            'contentType': 1,
            'cbContent': ''
          }, {
            'id': 1640,
            'contentId': null,
            'senceNo': '43',
            'content': '从技术到数据应用，我们不仅会产生一个专业的知识图谱，以及基于专用图谱之上的自然语言理解的技术，提升整个领域的生产力和行业效率。在后期数据清洗与算法优化的基础上，实现可视化的舆情数据报告和对趋势的智能化预测。',
            'level': 1,
            'parentId': 1636,
            'keyword': '(数据)',
            'description': '数据',
            'status': 2,
            'weight': null,
            'username': 'admin',
            'contentSign': 1615,
            'voiceId': null,
            'voice': null,
            'remark': null,
            'extendList': null,
            'commadList': [],
            'contentType': 1,
            'cbContent': ''
          }],
          'class': 'go.TreeModel'
        }
//      currentNode: null,
//      savedModelText: '',
//      counter: 1, // used by addNode
//      counter2: 4 // used by modifyStuff
      }
    },
    computed: {
//    currentNodeText: {
//      get: function () {
//        var node = this.currentNode
//        if (node instanceof go.Node) {
//          return node.data.text
//        } else {
//          return ''
//        }
//      },
//      set: function (val) {
//        var node = this.currentNode
//        if (node instanceof go.Node) {
//          var model = this.model()
//          model.startTransaction()
//          model.setDataProperty(node.data, 'text', val)
//          model.commitTransaction('edited text')
//        }
//      }
//    }
    },
    methods: {
//    getRichTextData (data) {
//      console.log(data)
//    },
      // get access to the GoJS Model of the GoJS Diagram
//    model: function () {
//      return this.$refs.diag.model()
//    },

      // tell the GoJS Diagram to update based on the arbitrarily modified model data
//    updateDiagramFromData: function () {
//      this.$refs.diag.updateDiagramFromData()
//    },

      // this event listener is declared on the <diagram>
      modelChanged: function (e) {
        if (e.isTransactionFinished) { // show the model data in the page's TextArea
          this.savedModelText = e.model.toJson()
        }
      },

      changedSelection: function (e) {
        console.log('changeselect')
        var node = e.diagram.selection.first()
        if (node instanceof go.Node) {
          this.currentNode = node
          this.currentNodeText = node.data.text
        } else {
          this.currentNode = null
          this.currentNodeText = ''
        }
      }

      // Here we modify the GoJS Diagram's Model using its methods,
      // which can be much more efficient than modifying some memory and asking
      // the GoJS Diagram to find differences and update accordingly.
      // Undo and Redo will work as expected.
//    addNode: function () {
//      var model = this.model()
//      model.startTransaction()
//      model.setDataProperty(model.findNodeDataForKey(4), 'color', 'purple')
//      var data = {
//        text: 'NEW ' + this.counter++,
//        color: 'yellow'
//      }
//      model.addNodeData(data)
//      model.addLinkData({
//        from: 3,
//        to: model.getKeyForNodeData(data)
//      })
//      model.commitTransaction('added Node and Link')
//      // also manipulate the Diagram by changing its Diagram.selection collection
//      var diagram = this.$refs.diag.diagram
//      diagram.select(diagram.findNodeForData(data))
//    },

      // Here we modify VUE's view model directly, and
      // then ask the GoJS Diagram to update everything from the data.
      // This is less efficient than calling the appropriate GoJS Model methods.
      // NOTE: Undo will not be able to restore all of the state properly!!
//    modifyStuff: function () {
//      var data = this.diagramData
//      data.nodeDataArray[0].color = 'red'
//      // Note here that because we do not have the GoJS Model,
//      // we cannot find out what values would be unique keys, for reference by the link data.
//      data.nodeDataArray.push({
//        key: ++this.counter2,
//        text: this.counter2.toString(),
//        color: 'orange'
//      })
//      data.linkDataArray.push({
//        from: 2,
//        to: this.counter2
//      })
//      this.updateDiagramFromData()
//    }
    }
  }
</script>

<style>

</style>
