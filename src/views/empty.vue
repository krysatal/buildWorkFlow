<template>
    <div class="containers" ref="content">
        <div class="canvas" ref="canvas"></div>
        <div id="js-properties-panel" class="panel"></div>
        <ul class="buttons">
            <li>下载</li>
            <li>
                <a ref="saveDiagram" href="javascript:" title="download BPMN diagram">BPMN diagram</a>
            </li>
            <li>
                <a ref="saveSvg" href="javascript:" title="download as SVG image">SVG image</a>
            </li>
        </ul>
    </div>
</template>

<script>
    import BpmnModeler from 'bpmn-js/lib/Modeler' // 建模器
    import propertiesPanelModule from 'bpmn-js-properties-panel' // 属性面板
    import propertiesProviderModule from 'bpmn-js-properties-panel/lib/provider/camunda' // 添加属性面板并编辑与执行相关的BPMN 2.0属性
    import camundaModdleDescriptor from 'camunda-bpmn-moddle/resources/camunda' //模块描述符
    import sketchyRendererModule from 'bpmn-js-sketchy' // 工作流的主题
    import translation from '../assets/customTranlate' // 汉化文件根据情况自己翻译
    export default {
        name: "empty",
        data () {
            return {
                bpmnModeler: null,
                container: null,
                canvas: null,
                xmlStr: null,
                processName: ''
            }
        },
        mounted() {
            // 获取到属性ref为“content”的dom节点
            this.container = this.$refs.content
            // 获取到属性ref为“canvas”的dom节点
            const canvas = this.$refs.canvas
            const customTranslateModule = {
                translate: [ 'value', translation ]
            };
            this.bpmnModeler = new BpmnModeler({
                container: canvas,
                // 添加控制板
                propertiesPanel: {
                    parent: '#js-properties-panel'
                },
                additionalModules: [
                    // 左边工具栏以及节点
                    propertiesProviderModule,
                    // 右边的工具栏
                    propertiesPanelModule,
                    sketchyRendererModule,
                    customTranslateModule
                ],
                moddleExtensions: {
                    camunda: camundaModdleDescriptor
                }
            })
            this.createNewDiagram(this.bpmnModeler);
        },
        methods: {
            createNewDiagram () {
                const bpmnXmlStr = '<?xml version="1.0" encoding="UTF-8"?>\n' +
                    '<bpmn2:definitions xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:bpmn2="http://www.omg.org/spec/BPMN/20100524/MODEL" xmlns:bpmndi="http://www.omg.org/spec/BPMN/20100524/DI" xmlns:dc="http://www.omg.org/spec/DD/20100524/DC" xmlns:di="http://www.omg.org/spec/DD/20100524/DI" xsi:schemaLocation="http://www.omg.org/spec/BPMN/20100524/MODEL BPMN20.xsd" id="sample-diagram" targetNamespace="http://bpmn.io/schema/bpmn">\n' +
                    '  <bpmn2:process id="Process_1" isExecutable="false">\n' +
                    '    <bpmn2:startEvent id="StartEvent_1"/>\n' +
                    '  </bpmn2:process>\n' +
                    '  <bpmndi:BPMNDiagram id="BPMNDiagram_1">\n' +
                    '    <bpmndi:BPMNPlane id="BPMNPlane_1" bpmnElement="Process_1">\n' +
                    '      <bpmndi:BPMNShape id="_BPMNShape_StartEvent_2" bpmnElement="StartEvent_1">\n' +
                    '        <dc:Bounds height="36.0" width="36.0" x="412.0" y="240.0"/>\n' +
                    '      </bpmndi:BPMNShape>\n' +
                    '    </bpmndi:BPMNPlane>\n' +
                    '  </bpmndi:BPMNDiagram>\n' +
                    '</bpmn2:definitions>'
                this.bpmnModeler.importXML(bpmnXmlStr, function (err) {
                    if (err) {
                        console.error(err)
                    }
                })
            }
        }
    }
</script>

<style lang="scss">
    @import '~bpmn-js/dist/assets/diagram-js.css';
    @import '~bpmn-js/dist/assets/bpmn-font/css/bpmn.css';
    @import '~bpmn-js/dist/assets/bpmn-font/css/bpmn-codes.css';
    @import '~bpmn-js/dist/assets/bpmn-font/css/bpmn-embedded.css';
    @import '~bpmn-js-properties-panel/dist/assets/bpmn-js-properties-panel.css';
    .containers {
        position: absolute;
        background-color: #ffffff;
        width: 100%;
        height: 90%;
    }

    .canvas {
        width: 100%;
        height: 100%;
    }

    .panel {
        position: absolute;
        right: 0;
        top: 0;
        width: 300px;
    }

    .buttons {
        position: absolute;
        left: 20px;
        bottom: 20px;

        & > li {
            display: inline-block;
            margin: 5px;

            & > a {
                color: #999;
                background: #eee;
                cursor: not-allowed;
                padding: 8px;
                border: 1px solid #ccc;

                &.active {
                    color: #333;
                    background: #fff;
                    cursor: pointer;
                }
            }
        }
    }
</style>