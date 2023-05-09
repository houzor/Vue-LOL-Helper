<template>
  <el-form-item label="客户端token" :label-width="formLabelWidth">
    <el-input v-model="token" autocomplete="off" placeholder="找到--remoting-auth-token=XXX"></el-input>
  </el-form-item>
  <el-row class="mb-4">
    <el-form-item>
      <el-button type="primary" style="margin-right: 10px;" @click="startgame">开启匹配房间</el-button>
    </el-form-item>
    <el-form-item>
      <el-button type="primary" @click="SendMyTeamMessage">一键查询队友战绩</el-button>
    </el-form-item>
  </el-row>
  <el-form>
    <el-row class="mb-4">
      <el-select v-model="rankedLeagueQueue" class="m-2" placeholder="选择游戏模式">
        <el-option v-for="item in mode" :key="item.value" :label="item.label" :value="item.value" />
      </el-select>
      <el-select v-model="rankedLeagueTier" class="m-2" placeholder="选择段位">
        <el-option v-for="item in tier" :key="item.value" :label="item.label" :value="item.value" />
      </el-select>
      <el-select v-model="rankedLeagueDivision" class="m-2" placeholder="选择级别">
        <el-option v-for="item in division" :key="item.value" :label="item.label" :value="item.value" />
      </el-select>
      <el-form-item>
        <el-button type="primary" @click="changeduanwei">更改段位</el-button>
      </el-form-item>
    </el-row>
  </el-form>
  <el-form>
    <el-row class="mb-4">
      <el-select v-model="availability" class="m-2" placeholder="选择游戏模式">
        <el-option v-for="item in status" :key="item.value" :label="item.label" :value="item.value" />
      </el-select>
      <el-form-item>
        <el-button type="primary" @click="changestatus">更改游戏状态</el-button>
      </el-form-item>
    </el-row>
  </el-form>
  <el-form>
    <el-row class="mb-4">
      <el-form-item label="皮肤编号" :label-width="formLabelWidth">
        <el-input style="margin-right: 10px;" v-model="pifu" autocomplete="off" placeholder="例99007"></el-input>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="changepifu">更改生涯界面皮肤</el-button>
      </el-form-item>
    </el-row>
  </el-form>
  <el-checkbox v-model="accept" @change="jieshou">自动接受对局(测试中)</el-checkbox>
</template>
<script setup>
import { reactive, onMounted, ref } from "vue"
import { ElMessage } from 'element-plus'
import axios from "axios"
const accept = ref(false)
const pifu = ref('')
const rankedLeagueQueue = ref('')
const rankedLeagueTier = ref('')
const rankedLeagueDivision = ref('')
const availability = ref('')
const status = [
  {
    value: 'chat',
    label: '在线',
  },
  {
    value: 'away',
    label: '离开',
  },
  {
    value: 'dnd',
    label: '游戏中',
  },
  {
    value: 'offline',
    label: '离线',
  },
  {
    value: 'mobile',
    label: '手机在线',
  },
]
const mode = [
  {
    value: 'RANKED_SOLO_5x5',
    label: '单排/双排',
  },
  {
    value: 'RANKED_FLEX_SR',
    label: '灵活组排 5v5',
  },
  {
    value: 'RANKED_FLEX_TT',
    label: '灵活组排 3v3',
  },
  {
    value: 'RANKED_TFT',
    label: '云顶之弈',
  },
]
const tier = [
  {
    value: 'IRON',
    label: '坚韧黑铁',
  },
  {
    value: 'BRONZE',
    label: '英勇黄铜',
  },
  {
    value: 'SILVER',
    label: '不屈白银',
  },
  {
    value: 'GOLD',
    label: '荣耀黄金',
  },
  {
    value: 'PLATINUM',
    label: '华贵铂金',
  },
  {
    value: 'DIAMOND',
    label: '璀璨钻石',
  },
  {
    value: 'MASTER',
    label: '超凡大师',
  },
  {
    value: 'GRANDMASTER',
    label: '傲世宗师',
  },
  {
    value: 'CHALLENGER',
    label: '最强王者',
  },
  {
    value: 'UNRANKED',
    label: '没有段位',
  },
]
const division = [
  {
    value: 'IV',
    label: '四',
  },
  {
    value: 'III',
    label: '三',
  },
  {
    value: 'II',
    label: '二',
  },
  {
    value: 'I',
    label: '一',
  }
]
var token = ref('')
async function jieshou() {
  while (accept.value) {
    await axios.get('/api/lol-matchmaking/v1/ready-check/accept',
      { auth: { username: 'riot', password: token.value } }, {
      headers: {
        'Content-Type': 'application/x-www-form-urlencoded;charset=UTF-8'
      }
    }).then((res) =>
      ElMessage({
        message: '操作成功',
        type: 'success',
      }));
  }
}
function sleep(delay) {
  var start = (new Date()).getTime();
  while ((new Date()).getTime() - start < delay) {
    continue;
  }
}

const startgame = () => {

  axios.post('/api/lol-lobby/v2/lobby',
    { queueId: 430 },
    { auth: { username: 'riot', password: token.value } }, {
    headers: {
      'Content-Type': 'application/x-www-form-urlencoded;charset=UTF-8'
    }
  }).then((res) =>
    ElMessage({
      message: '操作成功',
      type: 'success',
    }));
}

async function SendMyTeamMessage() {
  //查询出summonerId数组
  var shuzu = await axios.get('/api/lol-champ-select/v1/session', { auth: { username: 'riot', password: token.value } },
    {
      headers: {
        'Content-Type': 'application/x-www-form-urlencoded;charset=UTF-8'
      }
    })

  //查询出聊天室id
  var res1 = await axios.get('/api/lol-champ-select/v1/session',
    { auth: { username: 'riot', password: token.value } },
    {
      headers: {
        'Content-Type': 'application/x-www-form-urlencoded;charset=UTF-8'
      }
    })
  var myteam = shuzu.data.myTeam
  var theirteam = shuzu.data.theirTeam
  //遍历两个队的summonerId
  myteam.forEach(async (item, i) => {

    var info1 = await axios.get('/api/lol-summoner/v1/summoners/' + item.summonerId,
      { auth: { username: 'riot', password: token.value } },
      {
        headers: {
          'Content-Type': 'application/x-www-form-urlencoded;charset=UTF-8'
        }
      })

    var gamedata1 = await axios.get('/api/lol-match-history/v1/products/lol/' + info1.data.puuid + '/matches?begIndex=0&endIndex=5',
      { auth: { username: 'riot', password: token.value } },
      {
        headers: {
          'Content-Type': 'application/x-www-form-urlencoded;charset=UTF-8'
        }
      })
    //gamedata1.data.games.games[4].participantIdentities[0].player.summonerName
    // console.log(gamedata1.data.games.games[0].participants[0].stats.win)
    var myteamdata = (i + 1) + "楼最近五把的KDA为" +
      gamedata1.data.games.games[4].participants[0].stats.kills + '/' +
      gamedata1.data.games.games[4].participants[0].stats.deaths + '/' +
      gamedata1.data.games.games[4].participants[0].stats.assists + ' ' +
      gamedata1.data.games.games[3].participants[0].stats.kills + '/' +
      gamedata1.data.games.games[3].participants[0].stats.deaths + '/' +
      gamedata1.data.games.games[3].participants[0].stats.assists + ' ' +
      gamedata1.data.games.games[2].participants[0].stats.kills + '/' +
      gamedata1.data.games.games[2].participants[0].stats.deaths + '/' +
      gamedata1.data.games.games[2].participants[0].stats.assists + ' ' +
      gamedata1.data.games.games[1].participants[0].stats.kills + '/' +
      gamedata1.data.games.games[1].participants[0].stats.deaths + '/' +
      gamedata1.data.games.games[1].participants[0].stats.assists + ' ' +
      gamedata1.data.games.games[0].participants[0].stats.kills + '/' +
      gamedata1.data.games.games[0].participants[0].stats.deaths + '/' +
      gamedata1.data.games.games[0].participants[0].stats.assists + ' '
    var win = 0;

    for (var j = 0; j < 5; j++) {
      if (gamedata1.data.games.games[j].participants[0].stats.win == true) {
        win = win + 1;
      }
    }
    var shenglv = '胜率为' + win / 5 * 100
    var ma = ''
    if (win / 5 * 100 > 70) {
      ma = '鉴定为傲视群雄'
    }
    else if (win / 5 * 100 <= 70 && win / 5 * 100 > 60) {
      ma = '鉴定为上等马'
    } else if (win / 5 * 100 <= 60 && win / 5 * 100 > 50) {
      ma = '鉴定为中等马'
    } else if (win / 5 * 100 <= 50 && win / 5 * 100 > 40) {
      ma = '鉴定为下等马'
    } else if (win / 5 * 100 <= 40) {
      ma = '鉴定为牛马'
    }
    await axios.post('/api/lol-chat/v1/conversations/' + res1.data.chatDetails.multiUserChatId + '/messages',
      {
        body: myteamdata + shenglv + '%' + ' 鉴定为' + ma,
        type: "chat"
      },
      { auth: { username: 'riot', password: token.value } }, {
      headers: {
        'Content-Type': 'application/x-www-form-urlencoded;charset=UTF-8'
      }
    }).then((res) =>
      ElMessage({
        message: '操作成功',
        type: 'success',
      }));

  });

}
async function changeduanwei() {
  await axios.put('/api/lol-chat/v1/me',
    {
      lol: {
        rankedLeagueTier: rankedLeagueTier.value,
        rankedLeagueDivision: rankedLeagueDivision.value,
        rankedLeagueQueue: rankedLeagueQueue.value
      }
    },
    { auth: { username: 'riot', password: token.value } }, {
    headers: {
      'Content-Type': 'application/json;charset=UTF-8'
    }
  }).then((res) =>
    ElMessage({
      message: '操作成功',
      type: 'success',
    }));
}
async function changepifu() {
  await axios.post('/api/lol-summoner/v1/current-summoner/summoner-profile',
    {
      key: "backgroundSkinId",
      value: pifu.value
    },
    { auth: { username: 'riot', password: token.value } }, {
    headers: {
      'Content-Type': 'application/json;charset=UTF-8'
    }
  }).then((res) =>
    ElMessage({
      message: '操作成功',
      type: 'success',
    }));
}
async function changestatus() {
  await axios.put('/api/lol-chat/v1/me',
    {
      availability: availability.value
    },
    { auth: { username: 'riot', password: token.value } }, {
    headers: {
      'Content-Type': 'application/json;charset=UTF-8'
    }
  }).then((res) =>
    ElMessage({
      message: '操作成功',
      type: 'success',
    }));
}
</script>

<style scoped>
.m-2 {
  margin-right: 0.5rem;
}
</style>