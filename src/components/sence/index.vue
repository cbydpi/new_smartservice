<template>
  <div id="">
    <span style="position: relative;display: inline-block; vertical-align: top; width:10%;height: 100%;">
      <div id="myPaletteDiv" style="border: 1px solid #DBDBDB; height: 400px;"></div>
    </span>
    <span style="position: relative;display: inline-block; vertical-align: top; width:89%;height: 100%;">
      <div id='myDiagram' style='border: solid 1px black;width:100%;height:400px'></div>
      <div id="myOverviewDiv"></div> 
    </span>
  </div>
</template>

<script>
  import go from 'gojs'
  export default {

    props: ['modelData'],
    data () {
      return {
        diagram: null
      }
    },
    mounted () {
      var $ = go.GraphObject.make
      var self = this
//    console.log(this.$el)
      var myDiagram =
        $(go.Diagram, 'myDiagram', {
          initialContentAlignment: go.Spot.Center,
          maxSelectionCount: 1, // users can select only one part at a time
          validCycle: go.Diagram.CycleDestinationTree, // make sure users can only create trees
          allowDrop: true, // from Palette
          allowLink: false,
          allowClipboard: true,
          mouseDrop: function (e) {
            e.diagram.currentTool.doCancel()
//          finishDrop(e, true);
          },
          layout: $(go.TreeLayout, {
            treeStyle: go.TreeLayout.StyleLastParents,
            angle: 90,
            arrangement: go.TreeLayout.ArrangementHorizontal,
            alternateAngle: 90
          }),
          'undoManager.isEnabled': true,
          // Model ChangedEvents get passed up to component users
          'ModelChanged': function (e) {
//          console.log(e)
            self.$emit('model-changed', e)
          },
          'ChangedSelection': function (e) {
            self.$emit('changed-selection', e)
          }
        })

// 场景预览
      var myOverview =
        $(go.Overview, 'myOverviewDiv', {observed: myDiagram, contentAlignment: go.Spot.Center})

      function textStyle () {
        return {
          font: '12pt  Segoe UI,sans-serif',
          stroke: 'white'
        }
      }

      myDiagram.nodeTemplate =
        $(go.Node, 'Auto', {
          doubleClick: function (e, node, prev) {

          }
        }, {
          mouseDragEnter: function (e, node, prev) {

          },
          mouseDragLeave: function (e, node, next) {

          },
          mouseDrop: function (e, node) {

          }
        },
        $(go.Shape, 'Rectangle', {
          name: 'SHAPE',
          fill: '#18D2D1',
          stroke: null,
          portId: '',
          fromLinkable: false,
          toLinkable: false,
          cursor: 'pointer'
        }),
        $(go.Panel, 'Horizontal',
          $(go.Panel, 'Table', {
            margin: new go.Margin(0, 0, 0, 0),
            defaultAlignment: go.Spot.Left
          },
          $(go.RowColumnDefinition, {
            column: 5,
            width: 15
          }),
          $(go.TextBlock, '条件：', textStyle(), {
            row: 0,
            column: 0,
            margin: new go.Margin(8, 0, 8, 8)
          }),
          $(go.TextBlock, textStyle(),
            {
              wrap: go.TextBlock.WrapDesiredSize,
              row: 0,
              column: 1,
              columnSpan: 5,
              editable: false,
              isMultiline: false,
              margin: new go.Margin(8, 12, 8, 0),
              maxSize: new go.Size(250, 999)
            },
            new go.Binding('text', 'description').makeTwoWay()),
          $(go.TextBlock, '话术：', textStyle(), {
            row: 1,
            column: 0,
            margin: new go.Margin(0, 0, 12, 8)
          }),
          $(go.TextBlock, textStyle(),
            {
              wrap: go.TextBlock.WrapDesiredSize,
              row: 1,
              column: 1,
              columnSpan: 5,
              editable: false,
              isMultiline: false,
              margin: new go.Margin(0, 12, 12, 0),
              maxSize: new go.Size(250, 999)
            },
            new go.Binding('text', 'content').makeTwoWay())
          )
        ),
        $('TreeExpanderButton', {
          alignment: go.Spot.Bottom,
          alignmentFocus: go.Spot.Top
        }, {
          visible: true
        })
        )

      myDiagram.linkTemplate =
        $(go.Link, { selectable: false }, go.Link.Orthogonal, {
          corner: 5,
          relinkableFrom: true,
          relinkableTo: true
        },
        $(go.Shape, {
          strokeWidth: 4,
          stroke: '#00a4a4'
        })) // the link shape

      function getNextKey () {
        var key = nodeIdCounter
        while (myDiagram.model.findNodeDataForKey(key) !== null) {
          key = nodeIdCounter--
        }
        return key
      }

// 改变新老节点颜色
      myDiagram.layout.commitNodes = function () {
        go.TreeLayout.prototype.commitNodes.call(myDiagram.layout)
        myDiagram.layout.network.vertexes.each(function (v) {
          if (v.node) {
            if (v.node.data.id > -1) {
              var color = '#18D2D1'
            } else {
              var color = '#F0AD4E'
            }
            var shape = v.node.findObject('SHAPE')
            if (shape) {
              shape.fill = $(go.Brush, 'Linear', {
                0: color,
                1: go.Brush.lightenBy(color, 0.05),
                start: go.Spot.Left,
                end: go.Spot.Right
              })
            }
          }
        })
      }

      var nodeIdCounter = -1
      myDiagram.nodeTemplate.contextMenu =
        $(go.Adornment, 'Vertical',
          $('ContextMenuButton',
            $(go.TextBlock, '新增节点'), {
              click: function (e, obj) {
                var clicked = obj.part
                if (clicked !== null) {
                  var thisemp = clicked.data
                  myDiagram.startTransaction('add employee')
                  var newemp = {
                    id: getNextKey(),
                    content: '',
                    description: '',
                    parentId: thisemp.id
                  }
                  myDiagram.model.addNodeData(newemp)
                  myDiagram.commitTransaction('add employee')
                }
              }
            }
          ),
          $('ContextMenuButton',
            $(go.TextBlock, '删除此节点'), {
              click: function (e, obj) {
                if (obj.part.adornedPart.findTreeChildrenNodes().count > 0) {
                  self.$message({
                    type: 'warning',
                    message: '请先删除子节点!'
                  })
                  return
                }
//              myDiagram.commandHandler.deleteSelection()
                var data = {
                  diagram: myDiagram,
                  id: obj.part.data.id
                }
                self.$emit('deleteNode', data)
              }
            }
          ),
          $('ContextMenuButton',
            $(go.TextBlock, '编辑此节点'), {
              click: function (e, obj) {

              }
            }
          )
        )

// 左侧模板列表
      var myPalette =
        $(go.Palette, 'myPaletteDiv',  // must name or refer to the DIV HTML element
          {
            'animationManager.duration': 800 // slightly longer than default (600ms) animation
          })

      myPalette.nodeTemplateMap.add('', // the default category
        $(go.Node, 'Spot',
          $(go.Panel, 'Auto',
            $(go.Shape, 'Rectangle',
              { fill: '#18D2D1', stroke: null },
              new go.Binding('figure', 'figure')),
            $(go.TextBlock,
              {
                font: 'bold 11pt Helvetica, Arial, sans-serif',
                stroke: '#ffffff',
                margin: 8,
                maxSize: new go.Size(120, NaN),
                wrap: go.TextBlock.WrapFit,
                editable: true
              },
              new go.Binding('text').makeTwoWay())
          )
      ))

      var paletteData = [{
        'id': 40,
        'senceNo': null,
        'senceTitle': '4.从技术到数据应用',
        'useState': 0,
        'startTime': null,
        'endTime': null,
        'createTime': '2018-06-26 15:55:08',
        'creator': 'admin',
        'senceClassify': null,
        'submitTime': '2018-06-26 15:55:47',
        'checkTime': null,
        'submitter': 'admin',
        'checker': null,
        'classifys': [{
          'id': 7,
          'name': '奥法产品介绍',
          'parentid': -1,
          'level': null,
          'type': null,
          'group': 2
        }]
      }, {
        'id': 39,
        'senceNo': null,
        'senceTitle': '3.智能巡检',
        'useState': 0,
        'startTime': null,
        'endTime': null,
        'createTime': '2018-06-26 15:53:22',
        'creator': 'admin',
        'senceClassify': null,
        'submitTime': '2018-06-26 15:54:54',
        'checkTime': null,
        'submitter': 'admin',
        'checker': null,
        'classifys': [{
          'id': 7,
          'name': '奥法产品介绍',
          'parentid': -1,
          'level': null,
          'type': null,
          'group': 2
        }]
      }]
      paletteData = JSON.stringify(paletteData)
      paletteData = paletteData.replace(/id/g, 'key')
      paletteData = paletteData.replace(/senceTitle/g, 'text')
      paletteData = JSON.parse(paletteData)
      myPalette.model = new go.GraphLinksModel(paletteData)
// 模板列表完成
      this.diagram = myDiagram
      let updateModelData = JSON.stringify(this.modelData.nodeDataArray)
//    updateModelData = updateModelData.replace(/parentId/g, 'parent')
//    updateModelData = updateModelData.replace(/content/g, 'name')
//    updateModelData = updateModelData.replace(/id/g, 'key')
      this.modelData.nodeDataArray = JSON.parse(updateModelData)
//    myDiagram.model =
//      $(go.TreeModel,
//        { nodeParentKeyProperty: "parent",  // this property refers to the parent node data
//          nodeDataArray: this.modelData.nodeDataArray });
      this.updateModel(this.modelData)
//    console.log(this.modelData)
    },
    watch: {
      modelData: function (val) {
        this.updateModel(val)
      }
    },
    methods: {
      model: function () {
        return this.diagram.model
      },
      updateModel: function (val) {
        // No GoJS transaction permitted when replacing Diagram.model.
//      console.log(val)
        if (val instanceof go.Model) {
          this.diagram.model = val
        } else {
          var m = new go.TreeModel()
          m.nodeParentKeyProperty = 'parentId'
//        console.log(m.nodeKeyProperty)
          m.nodeKeyProperty = 'id'
          if (val) {
            for (var p in val) {
              m[p] = val[p]
            }
          }
          this.diagram.model = m
        }
      },
      updateDiagramFromData: function () {
        this.diagram.startTransaction()
        // This is very general but very inefficient.
        // It would be better to modify the diagramData data by calling
        // Model.setDataProperty or Model.addNodeData, et al.
        this.diagram.updateAllRelationshipsFromData()
        this.diagram.updateAllTargetBindings()
        this.diagram.commitTransaction('updated')
      }
    }
  }
</script>

<style>
#myOverviewDiv {
  position: absolute;
  width: 200px;
  height: 100px;
  top: 2px;
  left: 2px;
  background-color: aliceblue;
  z-index: 300;
  border: solid 1px blue;
}
</style>
