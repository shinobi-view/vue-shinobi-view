<template>
<div>
  <div class ="header">
    <h1><img id="img-logo" src="../assets/logo.png"> Shinobi-View</h1>
    <div class="auth-ui" v-if="disable.auth"><button id="authorize-button" @click="handleAuthClick">Google認証</button></div>
    <div class="auth-ui" v-if="disable.signout"><button id="signout-button" @click="handleSignoutClick">認証解除(Signout)</button></div>
    <div class="auth-ui" v-if="disable.signout"><button id="get-button" @click="getData">データ リフレッシュ</button></div>
    <div class="auth-ui">
      <label><input type="checkbox" v-model="filter"> 選択のみ表示</label>
    </div>
    <div style="flex-grow:1;"></div>
    <!-- <small class="header-caution">Meiryoだとずれるのは仕様</small> -->
  </div>
  <div class="main">
    <div class="arts" v-for="art in filteringData()" :key="art.id">
      <ninja-arts :arts="art" @toggle="toggleArts" />
    </div>
  </div>
</div>
</template>

<script>
import ninjaArts from '@/components/NinjaArts'

const disable = {
  auth: false,
  signout: false
}

const dataObj = {
  arts: [],
  select: [],
  disable,
  filter: false
}

const vm = {
  components: {
    'ninja-arts': ninjaArts
  },
  name: 'Main',
  data () {
    return dataObj
  },
  methods: {
    toggleArts (id, checked) {
      if (checked) {
        this.select.push(id)
      } else {
        this.select = this.select.filter(val => val !== id)
      }
      updateRoute(this.$router, this.select, this.filter)
    },
    /**
     *  Sign in the user upon button click.
     */
    handleAuthClick (event) {
      gapi.auth2.getAuthInstance().signIn()
    },
    /**
     *  Sign out the user upon button click.
     */
    handleSignoutClick (event) {
      gapi.auth2.getAuthInstance().signOut()
    },
    getData (event) {
      getData()
    },
    filteringData () {
      // OPTIMIZE: パフォーマンス問題のためにはtemplateから頻繁に呼ぶ仕様を修正する

      updateRoute(this.$router, this.select, this.filter)

      if (this.filter) {
        return this.arts.filter(val => {
          return this.select.includes(val.id)
        })
      } else {
        return this.arts
      }
    }
  },
  created () {
    const select = this.select
    if (this.$route.query) {
      const query = this.$route.query

      if (typeof query.select === 'string' && query.select.length > 0) {
        const sel = query.select.split(',')
        sel.forEach(function (val) {
          if (+val !== 0 && Number.isInteger(+val)) {
            select.push(val)
          }
        })
      }
      if (query.filter && query.filter === 'true') {
        this.filter = true
      }
    }

    try {
      if (localStorage && localStorage.getItem('arts')) {
        dataObj.arts = JSON.parse(localStorage.getItem('arts'))
      }
    } catch (error) {
      console.error(error)
    }

    gapi.load('client:auth2', initClient)
  }
}

export default vm

function updateRoute (router, select, filter) {
  router.push({
    // path: '',
    query: {
      select: select.join(),
      filter
    }
  })
}

/**
 *  Initializes the API client library and sets up sign-in state
 *  listeners.
 */
function initClient () {
  const CLIENT_ID = '1041910092103-p0e413imakhljr2rm4il6fc8rmrna0vd.apps.googleusercontent.com'

  // Array of API discovery doc URLs for APIs used by the quickstart
  const DISCOVERY_DOCS = ['https://sheets.googleapis.com/$discovery/rest?version=v4']

  // Authorization scopes required by the API; multiple scopes can be
  // included, separated by spaces.
  const SCOPES = 'https://www.googleapis.com/auth/spreadsheets.readonly'

  gapi.client.init({
    // apiKey: API_KEY,
    clientId: CLIENT_ID,
    discoveryDocs: DISCOVERY_DOCS,
    scope: SCOPES
  }).then(function () {
    // Listen for sign-in state changes.
    gapi.auth2.getAuthInstance().isSignedIn.listen(updateSigninStatus)

    // Handle the initial sign-in state.
    updateSigninStatus(gapi.auth2.getAuthInstance().isSignedIn.get())
  })
}

/**
 *  Called when the signed in status changes, to update the UI
 *  appropriately. After a sign-in, the API is called.
 */
function updateSigninStatus (isSignedIn) {
  if (isSignedIn) {
    disable.auth = true
    disable.signout = false
    if (dataObj.arts.length === 0) {
      getData()
    }
  } else {
    disable.auth = false
    disable.signout = true
  }
}

/**
 * Print the names and majors of students in a sample spreadsheet:
 * https://docs.google.com/spreadsheets/d/1BxiMVs0XRA5nFMdKvBdBZjgmUUqptlbs74OgvE2upms/edit
 */
function getData () {
  console.log('getData')
  gapi.client.sheets.spreadsheets.values.get({
    spreadsheetId: '10U2cJZZBNlWiYzl3UfXjLCmeb5c5-9BrTEFpvVRFdTQ',
    range: '\'忍術\'!A:K'
  }).then(function (response) {
    // vm.data.arts = parseArts(response.result.values)
    const arts = parseArts(response.result.values)
    dataObj.arts = arts

    if (localStorage) {
      localStorage.setItem('arts', JSON.stringify(arts))
    }
  }).catch(function (response) {
    console.error(response)
  })
}

/** データをオブジェクト形式に変換する */
function parseArts (values) {
  const correspondence = {
    '#': 'id',
    '勢力': 'power',
    '種類': 'type',
    '分家': 'brach',
    '忍術': 'name',
    'タイプ': 'type',
    '間合': 'interval',
    'コスト': 'cost',
    '指定特技': 'designatedSkill',
    '説明': 'description',
    '概要': 'sceneDescription'
  }

  const key = []

  values[0].forEach(elem => {
    if (correspondence[elem]) {
      key.push(correspondence[elem])
    } else {
      key.push(elem)
    }
  })

  const result = []

  values.slice(1).forEach((elem, idx) => {
    const obj = {}
    elem.forEach((k, m) => {
      obj[key[m]] = k
    })
    result[idx] = obj
  })

  return result
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1 {
  min-width: 250px;
}

#img-logo {
  width: 40px;
  height: 40px;
}

.header {
  display: flex;
  flex-direction: row;
  align-items: center;
  flex-wrap: wrap;
}

.header-caution {
  font-size: 8px;
  opacity: 40%;
}

.main {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
}

.arts {
  margin: 2px;
}

.auth-ui {
  margin: auto 4px;
}

.auth-ui label {
  font-size: 1.1rem;
}
.auth-ui input[type="checkbox"] {
  width: 1.2rem;
  height: 1.2rem;
  vertical-align: top;
}

.auth-ui > button {
  /* width: 120px;
  height: 40px; */

  /* color: rgb(233, 233, 233);
  font-size: large;
  border: 1px solid lightgray;
  background: linear-gradient(#646464, #202020); */
}
.auth-ui > button:disabled {
  color: rgb(141, 141, 141);
}
</style>
