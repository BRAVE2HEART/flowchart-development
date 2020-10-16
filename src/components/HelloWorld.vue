<template>
    <div class="hello">
        <div class="drag" id="div0" @click="onClicked" style="left:0;top:0;width:100px;height:100px">按住拖动</div>
        <!-- <div class="test" id="div-0">
            <span>Cloud018 said, </span>
            <span>"Hello World!!!"</span>
        </div> -->
    </div>
</template>
<script>
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

        window.onload = function () {
            var sourceNode = document.getElementById("div-0"); // 获得被克隆的节点对象 
            for (var i = 1; i < 5; i++) {
                var clonedNode = sourceNode.cloneNode(true); // 克隆节点 
                clonedNode.setAttribute("id", "div-" + i); // 修改一下id 值，避免id 重复 
                sourceNode.parentNode.appendChild(clonedNode); // 在父节点插入克隆的节点 
            }        }
    },
    methods: {
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
            this.copyUrl('string');
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
/* .hello {
    flex: 1;
    background: -webkit-linear-gradient(
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
    background-size: 81px 81px;
} */
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
