
<template>
    <div>
        <div class="folder_title">{{ data['folderName'] }}
            <el-button @click="folderUp"
                class="extra_buttons"><el-icon>
                    <ArrowUp />
                </el-icon></el-button>
            <el-button @click="folderDelete"
                class="extra_buttons"><el-icon>
                    <Minus />
                </el-icon></el-button>
        </div>
        <div class="list">
            <button @click="deleteMeme(item)" @dragenter="dragenter($event, index)" @dragover="dragover($event, index)"
                @dragstart="dragstart(index)" draggable="true" v-for="(item, index) in list" :key="item.memeName"
                class="list-item">
                <img class="meme" :src="item.src" />
            </button>
            <el-button @click="dialogFormVisible = true" class="extra_buttons" circle><el-icon>
                    <Plus />
                </el-icon></el-button>
        </div>
    </div>
    <!-- {{ list }} -->
    <el-dialog v-model="dialogFormVisible" title="新增表情">
        <el-input v-model="newMemeName" placeholder="名称 不可重复"></el-input>
        <el-input v-model="newMemeSrc" placeholder="直链地址"></el-input>
        <template #footer>
            <span class="dialog-footer">
                <el-button @click="dialogFormVisible = false">取消</el-button>
                <el-button type="primary" @click="newMeme" :disabled="!allowToCreate">
                    确认
                </el-button>
            </span>
        </template>
    </el-dialog>
</template>
<script setup>
import { computed, nextTick } from 'vue'
import { $computed } from 'vue/macros'
import { ElMessageBox } from 'element-plus'
var dialogFormVisible = $ref(false)
var newMemeName = $ref("")
var newMemeSrc = $ref("")
var allowToCreate = $computed(() => {
    var swit = true
    for (var i of list) {
        if (i['memeName'] == newMemeName) {
            swit = false
            break
        }
    }
    return newMemeName != "" && swit && /^https?:\/\/.+/.test(newMemeSrc)
})
var props = defineProps(['data'])
var emit = defineEmits(['folder-content-order-change', 'folder-rename', 'folder-delete','folder-up'])
var list = $computed(()=>{
    return props.data["content"]
})
var dragIndex = $ref('')

function newMeme() {
    dialogFormVisible = false
    list.push({
        "memeName": newMemeName,
        "src": newMemeSrc
    })
    newMemeName=""
    newMemeSrc=""
}
function deleteMeme(item) {
    for (var i of list) {
        if (i['memeName'] === item['memeName']) {
            list.pop(list.indexOf(i))

        }
    } emit('folder-content-order-change', {
        "folderName": props.data["folderName"],
        "content": list
    })

}
function folderUp(){
    emit('folder-up', props.data["folderName"])
}
function folderRename(newName) {
    emit('folder-rename', props.data["folderName"], newName)
}
function folderDelete() {
    console.log(1)
    emit('folder-delete', props.data["folderName"])
}
function dragstart(index) {
    dragIndex = index;
}
function dragenter(e, index) {
    e.preventDefault();
    // 避免源对象触发自身的dragenter事件
    if (dragIndex !== index) {
        const moving = list[dragIndex];
        list.splice(dragIndex, 1);
        list.splice(index, 0, moving);
        // 排序变化后目标对象的索引变成源对象的索引
        dragIndex = index;
        emit('folder-content-order-change', {
            "folderName": props.data["folderName"],
            "content": list
        })
    }
}
function dragover(e, index) {
    e.preventDefault();
}
</script>
<style lang="scss" scoped>
.folder_title {
    font-size: 1.6em;
    color: #667c99;
    text-align: left;
    margin: 0.25em 0;
}

.list {
    transition: all .3s;
    list-style: none;
    display: flex;
    flex-flow: row wrap;
    width: 300px;

    .list-move {
        transition: all .3s;
    }

    .list-item {
        transition: all .3s;
        width: 3em;
        background: transparent;
        overflow: hidden;
        border: none;
        color: #FFF;
        margin: 3px 3px;
        height: 3em;
        text-align: center;
    }
}

.meme {
    width: 3em;
    height: 3em;
}

.extra_buttons {
    margin: 3px 9px;

}
</style>