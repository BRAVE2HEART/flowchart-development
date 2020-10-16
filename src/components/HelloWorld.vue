<template>
    <div class="hello">
        <div id="mountNode">
        </div>
        <!-- <div class="drag" id="div0" @click="onClicked" style="left:0;top:0;width:100px;height:100px">按住拖动</div> -->
        <!-- <div class="test" id="div-0">
            <span>Cloud018 said, </span>
            <span>"Hello World!!!"</span>
        </div> -->
    </div>
</template>
<script>
import G6 from '@antv/g6';
export default {
    name: 'HelloWorld',
    props: {
        msg: String
    },
    data() {
        return {
            index: 1
        }
    },
    created() {

        // window.onload = function () {
        //     var sourceNode = document.getElementById("div-0"); // 获得被克隆的节点对象 
        //     for (var i = 1; i < 5; i++) {
        //         var clonedNode = sourceNode.cloneNode(true); // 克隆节点 
        //         clonedNode.setAttribute("id", "div-" + i); // 修改一下id 值，避免id 重复 
        //         sourceNode.parentNode.appendChild(clonedNode); // 在父节点插入克隆的节点 
        //     }        }

    },
    mounted() {
        this.initG6();
    },
    methods: {
        initG6() {
            const data = {
                nodes: [
                    {
                        id: 'node0',
                        x: 100,
                        y: 100,
                        size: 100,
                        // 该节点可选的连接点集合，该点有两个可选的连接点
                        anchorPoints: [
                            [0, 0.5],
                            [1, 0.5]],
                        linkPoints: {
                            top: true,
                            bottom: true,
                            left: true,
                            right: true,
                            fill: '#fff',
                            size: 10,
                            // ... 四个圆的样式可以在这里指定
                        },
                    },
                    {
                        id: 'node1',
                        x: 500,
                        y: 100,
                        size: 100,
                        // 该节点可选的连接点集合，该点有两个可选的连接点
                        anchorPoints: [
                            [0, 0.5],
                            [1, 0.5]
                        ],
                        linkPoints: {
                            top: true,
                            bottom: true,
                            left: true,
                            right: true,
                            fill: '#fff',
                            size: 10,
                            // ... 四个圆的样式可以在这里指定
                        },

                    },
                ],
                edges: [
                    {
                        // source: 'node0',
                        // target: 'node1',
                        // type: 'quadratic',
                        label: 'line',
                    },
                ],
            };
            console.log("created -> data", data)
            // const minimap = ...
            // 实例化 minimap 插件
            const minimap = new G6.Minimap({
                size: [100, 100],
                className: 'minimap',
                type: 'delegate',
            });
            // 实例化 grid 插件
            const grid = new G6.Grid();
            const graph = new G6.Graph({
                container: 'mountNode',
                width: 3000,
                height: 500,
                defaultNode: {
                    type: 'rect',
                },
                defaultEdge: {
                    type: 'cubic',  // 在数据中已经指定 type，这里无需再次指定
                    style: {
                        stroke: 'steelblue',
                        lineWidth: 5,
                        endArrow: false,
                        startArrow: false
                    },

                },
                modes: {
                    default: ['drag-canvas', 'zoom-canvas', 'drag-node'], // 允许拖拽画布、放缩画布、拖拽节点
                },
                // 节点不同状态下的样式集合
                nodeStateStyles: {
                    // 鼠标 hover 上节点，即 hover 状态为 true 时的样式
                    hover: {
                        fill: 'lightsteelblue',
                    },
                    // 鼠标点击节点，即 click 状态为 true 时的样式
                    click: {
                        stroke: '#000',
                        lineWidth: 3,
                    },
                },
                // 边不同状态下的样式集合
                edgeStateStyles: {
                    // 鼠标点击边，即 click 状态为 true 时的样式
                    click: {
                        stroke: 'steelblue',
                    },
                },
                plugins: [minimap, grid], // 将 grid 实例配置到图上
                animate: true, // Boolean，可选，全局变化时否使用动画过渡
            })
            console.log("created graph -> data", data)
            graph.data(data);
            graph.render();
            // 监听鼠标进入节点事件
            graph.on('node:mouseenter', (evt) => {
                const node = evt.item;
                // 激活该节点的 hover 状态
                graph.setItemState(node, 'hover', true);
            });
            // 监听鼠标离开节点事件
            graph.on('node:mouseleave', (evt) => {
                const node = evt.item;
                // 关闭该节点的 hover 状态
                graph.setItemState(node, 'hover', false);
            });
        },
        copyUrl(url) {
            var domUrl = document.createElement("input");
            domUrl.value = url;
            domUrl.id = "creatDom";
            document.body.appendChild(domUrl);
            domUrl.select(); // 选择对象
            document.execCommand("Copy"); // 执行浏览器复制命令
            let creatDom = document.getElementById("creatDom");
            creatDom.parentNode.removeChild(creatDom);
            alert("已复制好，可贴粘。");
        },
        onClicked() {
            // 获取DOM元素  
            // var sourceNode = document.getElementById("div0"); // 获得被克隆的节点对象 
            // var clonedNode = sourceNode.cloneNode(true); // 克隆节点 
            // clonedNode.setAttribute("id", "div1"); // 修改一下id 值，避免id 重复 
            // // clonedNode.setAttribute("class","drag")
            // sourceNode.parentNode.appendChild(clonedNode); // 在父节点插入克隆的节点 
            //  let dragDiv = document.getElementsByClassName("drag")[0];
            // this.copyUrl('string');
            let dragDiv = document.getElementsByClassName("drag")[0];
            // 鼠标按下事件 处理程序
            let putDown = function (event) {
                dragDiv.style.cursor = "pointer";
                let offsetX = parseInt(dragDiv.style.left); // 获取当前的x轴距离
                let offsetY = parseInt(dragDiv.style.top); // 获取当前的y轴距离
                let innerX = event.clientX - offsetX; // 获取鼠标在方块内的x轴距
                let innerY = event.clientY - offsetY; // 获取鼠标在方块内的y轴距
                // 按住鼠标时为div添加一个border
                dragDiv.style.borderStyle = "solid";
                dragDiv.style.borderColor = "red";
                dragDiv.style.borderWidth = "3px";
                // 鼠标移动的时候不停的修改div的left和top值
                document.onmousemove = function (event) {
                    dragDiv.style.left = event.clientX - innerX + "px";
                    dragDiv.style.top = event.clientY - innerY + "px";
                    // 边界判断
                    if (parseInt(dragDiv.style.left) <= 0) {
                        dragDiv.style.left = "0px";
                    }
                    if (parseInt(dragDiv.style.top) <= 0) {
                        dragDiv.style.top = "0px";
                    }
                    if (parseInt(dragDiv.style.left) >= window.innerWidth - parseInt(dragDiv.style.width)) {
                        dragDiv.style.left = window.innerWidth - parseInt(dragDiv.style.width) + "px";
                    }
                    if (parseInt(dragDiv.style.top) >= window.innerHeight - parseInt(dragDiv.style.height)) {
                        dragDiv.style.top = window.innerHeight - parseInt(dragDiv.style.height) + "px";
                    }
                }
                // 鼠标抬起时，清除绑定在文档上的mousemove和mouseup事件
                // 否则鼠标抬起后还可以继续拖拽方块
                document.onmouseup = function () {
                    document.onmousemove = null;
                    document.onmouseup = null;
                    // 清除border
                    dragDiv.style.borderStyle = "";
                    dragDiv.style.borderColor = "";
                    dragDiv.style.borderWidth = "";
                }
            }
            // 绑定鼠标按下事件
            dragDiv.addEventListener("mousedown", putDown, false);
        }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.hello {
    flex: 1;
    /* background-color: antiquewhite; */
    /* background: -webkit-linear-gradient(
            top,
            transparent 39px,
            blue 39px,
            blue 41px,
            transparent 41px,
            transparent 79px,
            red 80px
        ),
        -webkit-linear-gradient(left, transparent 39px, orange 39px, orange 41px, transparent
                    41px, transparent 79px, green 80px);
    background-size: 81px 81px; */
}
.drag {
    background-color: skyblue;
    position: absolute;
    line-height: 100px;
    text-align: center;
}
h3 {
    margin: 40px 0 0;
}
ul {
    list-style-type: none;
    padding: 0;
}
li {
    display: inline-block;
    margin: 0 10px;
}
a {
    color: #42b983;
}
</style>
