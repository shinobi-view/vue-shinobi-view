<template>
<div>
  <button id="authorize-button" @click="handleAuthClick" v-bind:disabled="disable.auth">authorize</button>
  <button id="signout-button" @click="handleSignoutClick" v-bind:disabled="disable.signout">signout</button>
  <div class="main">
    <div class="arts" v-for="art in arts" :key="art.id">
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
  disable
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
    toggleArts (checked) {
      console.log(checked)
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
    }
  },
  created () {
    gapi.load('client:auth2', initClient)
  }
}

export default vm

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
    getData()
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
  gapi.client.sheets.spreadsheets.values.get({
    spreadsheetId: '10U2cJZZBNlWiYzl3UfXjLCmeb5c5-9BrTEFpvVRFdTQ',
    range: '\'忍術\'!A:K'
  }).then(function (response) {
    // vm.data.arts = parseArts(response.result.values)
    const arts = parseArts(response.result.values)
    dataObj.arts = arts
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
.main {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
}
.arts {
  margin: 2px;
}
</style>
