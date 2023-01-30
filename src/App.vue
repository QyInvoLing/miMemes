<template>
  <main>
    <div>
      <el-alert title="拖拽表情以进行排序，点击表情删除" type="info" show-icon :closable="false" />
      <el-alert title="修改后需要手动保存" type="info" show-icon :closable="false" />
      <el-alert title="点击查询使用说明" @click="help" type="info" show-icon :closable="false" />
      <div>
        <el-button @click="loadConfig">重置</el-button><el-button @click="saveConfig">保存</el-button>
      </div>
      <memesFolder v-for="(item, index) in memesConfig" :key="item.memeName" class="list-item" :data="item"
        @folder-content-order-change="folderChange" @folder-up="folderUp" @folder-delete="folderDelete"></memesFolder>
      <el-button @click="dialogFormVisible = true" class="new_folder"><el-icon>
          <Plus />
          
        </el-icon> 新建文件夹</el-button>
    </div>

  </main>
  <p :hidden="!devMode">{{ memesConfig }}</p>
  <el-dialog v-model="dialogFormVisible" title="新建文件夹">
    <el-input v-model="newFolderName" placeholder="名称 不可重复"></el-input>
    <template #footer>
      <span class="dialog-footer">
        <el-button @click="dialogFormVisible = false">取消</el-button>
        <el-button type="primary" @click="folderNew" :disabled="!allowToCreate">
          确认
        </el-button>
      </span>
    </template>
  </el-dialog>
</template>

<script setup>
import { onMounted, ref, computed } from 'vue'
import { $ref, $computed } from 'vue/macros'
import memesFolder from './components/memesFolder.vue'
onMounted(() => {
  loadConfig()
})

var memesConfig = $ref([])
var devMode = $ref(false)
var dialogFormVisible = $ref(false)
var newFolderName = $ref("")
var allowToCreate = $computed(() => {
  var swit = true
  for (var i of memesConfig) {
    if (i['folderName'] == newFolderName) {
      swit = false
      break
    }
  }
  return newFolderName != "" && swit
})
function help() {
  window.open("https://michan.fun/d/407/2")
}
function folderUp(folderName){
  for (var i in memesConfig) {
    if (memesConfig[i]['folderName'] == folderName && i!=0) {
      var temp = memesConfig[i]
      memesConfig[i] = memesConfig[i-1]
      memesConfig[i-1] = temp
    }
  }
}
function folderDelete(folderName) {
  for (var i of memesConfig) {
    if (i['folderName'] == folderName) {
      memesConfig.pop(memesConfig.indexOf(i))
    }
  }
}
function folderRename(folderName, newName) {
  for (var i of memesConfig) {
    if (i['folderName'] == folderName) {
      memesConfig[memesConfig.indexOf(i)]["folderName"] = newName
    }
  }
}
function folderChange(data) {
  console.log(data)
  for (var i of memesConfig) {
    if (i['folderName'] == data['folderName']) {
      memesConfig[memesConfig.indexOf(i)] = data
    }
  }
  console.log(memesConfig)
}
function folderNew() {
  dialogFormVisible = false
  memesConfig.push({ "folderName": newFolderName, "content": [] })
  newFolderName = ""
}
function loadConfig() {
  if (!chrome.storage) {//dev mode
    console.log("Running in dev mode...")
    devMode = true
    memesConfig = [{
      "folderName": "原神AC娘",
      "content": [
        {
          "memeName": "神里绫华",
          "src": "https://i.ibb.co/K64VW28/image.gif"
        },
        {
          "memeName": "班尼特",
          "src": "https://i.ibb.co/kKGLxg0/image.gif"
        },
        {
          "memeName": "可莉",
          "src": "https://i.ibb.co/4S4qZR3/image.gif"
        },
        {
          "memeName": "珊瑚宫心海",
          "src": "https://i.ibb.co/vP1THVX/ci.gif"
        },
        {
          "memeName": "八重神子",
          "src": "https://i.ibb.co/SdhZ01y/image.gif"
        },
        {
          "memeName": "阿贝多",
          "src": "https://i.ibb.co/VNXjrCz/image.gif"
        },
        {
          "memeName": "刻晴",
          "src": "https://i.ibb.co/6BH4rzZ/image.gif"
        },
        {
          "memeName": "神里绫人",
          "src": "https://i.ibb.co/ZVLVLf5/image.gif"
        },

        {
          "memeName": "甘雨",
          "src": "https://i.ibb.co/WsswMqR/image.gif"
        },
        {
          "memeName": "早柚",
          "src": "https://i.ibb.co/sCZDLXV/image.gif"
        },
        {
          "memeName": "胡桃",
          "src": "https://i.ibb.co/ctP4yRQ/image.gif"
        },
        {
          "memeName": "温迪",
          "src": "https://i.ibb.co/QvZq7pS/image.gif"
        },
        {
          "memeName": "达达利亚",
          "src": "https://i.ibb.co/2Sm6jDC/3G.gif"
        },
        {
          "memeName": "申鹤",
          "src": "https://i.ibb.co/0stnzxq/image.gif"
        },
        {
          "memeName": "烟绯",
          "src": "https://i.ibb.co/0nD11Pg/image.gif"
        },
        {
          "memeName": "迪卢克",
          "src": "https://i.ibb.co/0m2FsdY/image.gif"
        },
        {
          "memeName": "枫原万叶",
          "src": "https://i.ibb.co/pxb2nyD/image.gif"
        },
        {
          "memeName": "钟离",
          "src": "https://i.ibb.co/47s803f/image.gif"
        },
        {
          "memeName": "国崩",
          "src": "https://i.ibb.co/1MJ8wdc/image.gif"
        },
        {
          "memeName": "芭芭拉",
          "src": "https://i.ibb.co/0cThFVW/image.gif"
        },
        {
          "memeName": "菲谢尔",
          "src": "https://i.ibb.co/yYZ2TKY/image.gif"
        },
        {
          "memeName": "白术",
          "src": "https://i.ibb.co/qnD4qnB/image.gif"
        },
        {
          "memeName": "凯亚",
          "src": "https://i.ibb.co/HqRv9fJ/image.gif"
        },
        {
          "memeName": "雷电将军",
          "src": "https://i.ibb.co/VJdZ65y/image.gif"
        },
        {
          "memeName": "荒泷一斗",
          "src": "https://i.ibb.co/4tMJPPk/image.gif"
        },
        {
          "memeName": "香菱",
          "src": "https://i.ibb.co/GJ0m23c/image.gif"
        },
        {
          "memeName": "魈",
          "src": "https://i.ibb.co/nmhpYHc/image.gif"
        },
        {
          "memeName": "莫娜",
          "src": "https://i.ibb.co/zXdrbHG/image.gif"
        },
        {
          "memeName": "九条裟罗",
          "src": "https://i.ibb.co/Sm7bD6S/image.gif"
        },
        {
          "memeName": "托马",
          "src": "https://i.ibb.co/YBnVXTm/w.gif"
        },
        {
          "memeName": "安柏",
          "src": "https://i.ibb.co/FHTKVS2/image.gif"
        },
        {
          "memeName": "行秋",
          "src": "https://i.ibb.co/Pcsg7kT/image.gif"
        },
        {
          "memeName": "重云",
          "src": "https://i.ibb.co/vJCY5Fr/image.gif"
        },
        {
          "memeName": "砂糖",
          "src": "https://i.ibb.co/2FMfvPj/image.gif"
        },
        {
          "memeName": "五郎",
          "src": "https://i.ibb.co/ByLTV7Q/image.gif"
        },
        {
          "memeName": "凝光",
          "src": "https://i.ibb.co/QFc0Hk6/image.gif"
        },
        {
          "memeName": "迪奥娜",
          "src": "https://i.ibb.co/y0F0yKB/image.gif"
        },
        {
          "memeName": "云堇",
          "src": "https://i.ibb.co/X2KjK1D/image.gif"
        },
        {
          "memeName": "优菈",
          "src": "https://i.ibb.co/B6pjyV6/image.gif"
        },
        {
          "memeName": "宵宫",
          "src": "https://i.ibb.co/m0SDKDd/image.gif"
        },
        {
          "memeName": "琴",
          "src": "https://i.ibb.co/8K6xp6W/image.gif"
        },
        {
          "memeName": "诺艾尔",
          "src": "https://i.ibb.co/6BH2Y06/image.gif"
        },
        {
          "memeName": "丽莎",
          "src": "https://i.ibb.co/Wck4xN5/image.gif"
        },
        {
          "memeName": "雷泽",
          "src": "https://i.ibb.co/3zCR41B/image.gif"
        },
        {
          "memeName": "北斗",
          "src": "https://i.ibb.co/3prrygZ/image.gif"
        },
        {
          "memeName": "罗莎莉亚",
          "src": "https://i.ibb.co/YNyktZf/image.gif"
        },
        {
          "memeName": "辛焱",
          "src": "https://i.ibb.co/7N3g72Q/image.gif"
        },
        {
          "memeName": "瑶瑶",
          "src": "https://i.ibb.co/7gPKcDK/image.gif"
        },
        {
          "memeName": "戴因斯雷布",
          "src": "https://i.ibb.co/XtQ0yqJ/image.gif"
        },
        {
          "memeName": "空",
          "src": "https://i.ibb.co/G0sJsfW/image.gif"
        },
        {
          "memeName": "荧",
          "src": "https://i.ibb.co/ncYgbqL/image.gif"
        },
        {
          "memeName": "空-舔",
          "src": "https://i.ibb.co/QQ7LGJX/image.gif"
        },
        {
          "memeName": "荧-喘",
          "src": "https://i.ibb.co/zZy3R5C/image.gif"
        },
        {
          "memeName": "空-干杯",
          "src": "https://i.ibb.co/DYPHKLw/image.gif"
        },
        {
          "memeName": "荧-干杯",
          "src": "https://i.ibb.co/LpG7Y99/image.gif"
        },
        {
          "memeName": "派蒙-耶",
          "src": "https://i.ibb.co/StPqx6s/image.gif"
        },
        {
          "memeName": "派蒙-指",
          "src": "https://i.ibb.co/m0t3m9w/image.gif"
        },
        {
          "memeName": "派蒙-吃",
          "src": "https://i.ibb.co/BwpfHn8/image.gif"
        },
        {
          "memeName": "久岐忍",
          "src": "https://i.ibb.co/Yj2yFP2/image.png"
        },
        {
          "memeName": "夜兰",
          "src": "https://i.ibb.co/4VRNGdm/image.png"
        },
        {
          "memeName": "鹿野苑平藏",
          "src": "https://i.ibb.co/1ZLd160/image.png"
        },
        {
          "memeName": "迪卢克2",
          "src": "https://i.ibb.co/NCPnpTV/image.gif"
        },
        {
          "memeName": "多莉",
          "src": "https://i.ibb.co/6Y9WcWT/image.gif"
        },
        {
          "memeName": "提纳里",
          "src": "https://i.ibb.co/XpsTc5M/image.gif"
        },
        {
          "memeName": "少女",
          "src": "https://i.ibb.co/QjVNxvH/image.gif"
        },
        {
          "memeName": "妮露",
          "src": "https://i.ibb.co/svvnJ8n/image.gif"
        },
        {
          "memeName": "赛诺",
          "src": "https://i.ibb.co/xfvnt9j/image.gif"
        },
        {
          "memeName": "富人",
          "src": "https://i.ibb.co/tDwZMm1/image.gif"
        },
        {
          "memeName": "纳西妲",
          "src": "https://i.ibb.co/sg5zPpd/A.gif"
        },
        {
          "memeName": "纳西妲-啊",
          "src": "https://i.ibb.co/sChyFTY/B-wakuwaku.gif"
        },
        {
          "memeName": "坎蒂丝-抱荧",
          "src": "https://i.ibb.co/W29N99g/image.gif"
        },
        {
          "memeName": "坎蒂丝-抱空",
          "src": "https://i.ibb.co/wLP6P04/image.gif"
        },
        {
          "memeName": "坎蒂丝-珐露珊",
          "src": "https://i.ibb.co/ZBtzFC3/image.gif"
        },
        {
          "memeName": "那个女人",
          "src": "https://i.ibb.co/dJ6199b/image.gif"
        },
        {
          "memeName": "归终",
          "src": "https://i.ibb.co/pP14JTb/image.gif"
        },
        {
          "memeName": "阿萍",
          "src": "https://i.ibb.co/GVLghBh/image.gif"
        },

      ]
    },
    {
      "folderName": "用户自定义表情",
      "content": [
        {
          "memeName": "Chrome",
          "src": "https://www.runoob.com/images/compatible_chrome.gif"
        }
      ]
    }]
    console.log(memesConfig)
    return
  }
  console.log("Running in production mode...")
  chrome.storage.local.get("memesConfig").then((config) => {
    memesConfig = [{
      "folderName": "原神AC娘",
      "content": [
        {
          "memeName": "神里绫华",
          "src": "https://i.ibb.co/K64VW28/image.gif"
        },
        {
          "memeName": "班尼特",
          "src": "https://i.ibb.co/kKGLxg0/image.gif"
        },
        {
          "memeName": "可莉",
          "src": "https://i.ibb.co/4S4qZR3/image.gif"
        },
        {
          "memeName": "珊瑚宫心海",
          "src": "https://i.ibb.co/vP1THVX/ci.gif"
        },
        {
          "memeName": "八重神子",
          "src": "https://i.ibb.co/SdhZ01y/image.gif"
        },
        {
          "memeName": "阿贝多",
          "src": "https://i.ibb.co/VNXjrCz/image.gif"
        },
        {
          "memeName": "刻晴",
          "src": "https://i.ibb.co/6BH4rzZ/image.gif"
        },
        {
          "memeName": "神里绫人",
          "src": "https://i.ibb.co/ZVLVLf5/image.gif"
        },

        {
          "memeName": "甘雨",
          "src": "https://i.ibb.co/WsswMqR/image.gif"
        },
        {
          "memeName": "早柚",
          "src": "https://i.ibb.co/sCZDLXV/image.gif"
        },
        {
          "memeName": "胡桃",
          "src": "https://i.ibb.co/ctP4yRQ/image.gif"
        },
        {
          "memeName": "温迪",
          "src": "https://i.ibb.co/QvZq7pS/image.gif"
        },
        {
          "memeName": "达达利亚",
          "src": "https://i.ibb.co/2Sm6jDC/3G.gif"
        },
        {
          "memeName": "申鹤",
          "src": "https://i.ibb.co/0stnzxq/image.gif"
        },
        {
          "memeName": "烟绯",
          "src": "https://i.ibb.co/0nD11Pg/image.gif"
        },
        {
          "memeName": "迪卢克",
          "src": "https://i.ibb.co/0m2FsdY/image.gif"
        },
        {
          "memeName": "枫原万叶",
          "src": "https://i.ibb.co/pxb2nyD/image.gif"
        },
        {
          "memeName": "钟离",
          "src": "https://i.ibb.co/47s803f/image.gif"
        },
        {
          "memeName": "国崩",
          "src": "https://i.ibb.co/1MJ8wdc/image.gif"
        },
        {
          "memeName": "芭芭拉",
          "src": "https://i.ibb.co/0cThFVW/image.gif"
        },
        {
          "memeName": "菲谢尔",
          "src": "https://i.ibb.co/yYZ2TKY/image.gif"
        },
        {
          "memeName": "白术",
          "src": "https://i.ibb.co/qnD4qnB/image.gif"
        },
        {
          "memeName": "凯亚",
          "src": "https://i.ibb.co/HqRv9fJ/image.gif"
        },
        {
          "memeName": "雷电将军",
          "src": "https://i.ibb.co/VJdZ65y/image.gif"
        },
        {
          "memeName": "荒泷一斗",
          "src": "https://i.ibb.co/4tMJPPk/image.gif"
        },
        {
          "memeName": "香菱",
          "src": "https://i.ibb.co/GJ0m23c/image.gif"
        },
        {
          "memeName": "魈",
          "src": "https://i.ibb.co/nmhpYHc/image.gif"
        },
        {
          "memeName": "莫娜",
          "src": "https://i.ibb.co/zXdrbHG/image.gif"
        },
        {
          "memeName": "九条裟罗",
          "src": "https://i.ibb.co/Sm7bD6S/image.gif"
        },
        {
          "memeName": "托马",
          "src": "https://i.ibb.co/YBnVXTm/w.gif"
        },
        {
          "memeName": "安柏",
          "src": "https://i.ibb.co/FHTKVS2/image.gif"
        },
        {
          "memeName": "行秋",
          "src": "https://i.ibb.co/Pcsg7kT/image.gif"
        },
        {
          "memeName": "重云",
          "src": "https://i.ibb.co/vJCY5Fr/image.gif"
        },
        {
          "memeName": "砂糖",
          "src": "https://i.ibb.co/2FMfvPj/image.gif"
        },
        {
          "memeName": "五郎",
          "src": "https://i.ibb.co/ByLTV7Q/image.gif"
        },
        {
          "memeName": "凝光",
          "src": "https://i.ibb.co/QFc0Hk6/image.gif"
        },
        {
          "memeName": "迪奥娜",
          "src": "https://i.ibb.co/y0F0yKB/image.gif"
        },
        {
          "memeName": "云堇",
          "src": "https://i.ibb.co/X2KjK1D/image.gif"
        },
        {
          "memeName": "优菈",
          "src": "https://i.ibb.co/B6pjyV6/image.gif"
        },
        {
          "memeName": "宵宫",
          "src": "https://i.ibb.co/m0SDKDd/image.gif"
        },
        {
          "memeName": "琴",
          "src": "https://i.ibb.co/8K6xp6W/image.gif"
        },
        {
          "memeName": "诺艾尔",
          "src": "https://i.ibb.co/6BH2Y06/image.gif"
        },
        {
          "memeName": "丽莎",
          "src": "https://i.ibb.co/Wck4xN5/image.gif"
        },
        {
          "memeName": "雷泽",
          "src": "https://i.ibb.co/3zCR41B/image.gif"
        },
        {
          "memeName": "北斗",
          "src": "https://i.ibb.co/3prrygZ/image.gif"
        },
        {
          "memeName": "罗莎莉亚",
          "src": "https://i.ibb.co/YNyktZf/image.gif"
        },
        {
          "memeName": "辛焱",
          "src": "https://i.ibb.co/7N3g72Q/image.gif"
        },
        {
          "memeName": "瑶瑶",
          "src": "https://i.ibb.co/7gPKcDK/image.gif"
        },
        {
          "memeName": "戴因斯雷布",
          "src": "https://i.ibb.co/XtQ0yqJ/image.gif"
        },
        {
          "memeName": "空",
          "src": "https://i.ibb.co/G0sJsfW/image.gif"
        },
        {
          "memeName": "荧",
          "src": "https://i.ibb.co/ncYgbqL/image.gif"
        },
        {
          "memeName": "空-舔",
          "src": "https://i.ibb.co/QQ7LGJX/image.gif"
        },
        {
          "memeName": "荧-喘",
          "src": "https://i.ibb.co/zZy3R5C/image.gif"
        },
        {
          "memeName": "空-干杯",
          "src": "https://i.ibb.co/DYPHKLw/image.gif"
        },
        {
          "memeName": "荧-干杯",
          "src": "https://i.ibb.co/LpG7Y99/image.gif"
        },
        {
          "memeName": "派蒙-耶",
          "src": "https://i.ibb.co/StPqx6s/image.gif"
        },
        {
          "memeName": "派蒙-指",
          "src": "https://i.ibb.co/m0t3m9w/image.gif"
        },
        {
          "memeName": "派蒙-吃",
          "src": "https://i.ibb.co/BwpfHn8/image.gif"
        },
        {
          "memeName": "久岐忍",
          "src": "https://i.ibb.co/Yj2yFP2/image.png"
        },
        {
          "memeName": "夜兰",
          "src": "https://i.ibb.co/4VRNGdm/image.png"
        },
        {
          "memeName": "鹿野苑平藏",
          "src": "https://i.ibb.co/1ZLd160/image.png"
        },
        {
          "memeName": "迪卢克2",
          "src": "https://i.ibb.co/NCPnpTV/image.gif"
        },
        {
          "memeName": "多莉",
          "src": "https://i.ibb.co/6Y9WcWT/image.gif"
        },
        {
          "memeName": "提纳里",
          "src": "https://i.ibb.co/XpsTc5M/image.gif"
        },
        {
          "memeName": "少女",
          "src": "https://i.ibb.co/QjVNxvH/image.gif"
        },
        {
          "memeName": "妮露",
          "src": "https://i.ibb.co/svvnJ8n/image.gif"
        },
        {
          "memeName": "赛诺",
          "src": "https://i.ibb.co/xfvnt9j/image.gif"
        },
        {
          "memeName": "富人",
          "src": "https://i.ibb.co/tDwZMm1/image.gif"
        },
        {
          "memeName": "纳西妲",
          "src": "https://i.ibb.co/sg5zPpd/A.gif"
        },
        {
          "memeName": "纳西妲-啊",
          "src": "https://i.ibb.co/sChyFTY/B-wakuwaku.gif"
        },
        {
          "memeName": "坎蒂丝-抱荧",
          "src": "https://i.ibb.co/W29N99g/image.gif"
        },
        {
          "memeName": "坎蒂丝-抱空",
          "src": "https://i.ibb.co/wLP6P04/image.gif"
        },
        {
          "memeName": "坎蒂丝-珐露珊",
          "src": "https://i.ibb.co/ZBtzFC3/image.gif"
        },
        {
          "memeName": "那个女人",
          "src": "https://i.ibb.co/dJ6199b/image.gif"
        },
        {
          "memeName": "归终",
          "src": "https://i.ibb.co/pP14JTb/image.gif"
        },
        {
          "memeName": "阿萍",
          "src": "https://i.ibb.co/GVLghBh/image.gif"
        },

      ]
    },
    {
      "folderName": "用户自定义表情",
      "content": [
        {
          "memeName": "Chrome",
          "src": "https://www.runoob.com/images/compatible_chrome.gif"
        }
      ]
    }]
    if (config.memesConfig) {
      console.log("Found existed config.")
      memesConfig = []
      console.log(config)
      for (var i in config.memesConfig) {//重整被storage去掉的array
        config.memesConfig[i]['content'] = Object.keys(config.memesConfig[i]['content']).map((x) => {
          return config.memesConfig[i]['content'][x]
        })
        memesConfig.push(config.memesConfig[i])
      }

    }
    console.log(memesConfig)
  })
}
function saveConfig() {
  chrome.storage.local.set({ "memesConfig": memesConfig }, () => { })
}


</script>



<style scoped>
@media (min-width: 1024px) {
  main {
    margin: 0 auto;
  }

  .el-alert {
    margin-top: 1em;
    margin-bottom: 1em;
  }

  .list-item {
    transition: all 0.3s;
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }

  .new_folder {
    margin-top: 2em;
  }
}
</style>
