<template>
  <div class="frame outer">

    <div class="arts-title">
      <div class="arts-name">{{arts.name}}</div>
      <div class="arts-name-kana">{{arts.nameKana}}</div>
      <div style="flex-grow:1;"></div>
      <div><input type="checkbox" class="arts-checkbox" v-model="checked"  @click="toggle(arts.id,checked)"></div>
    </div>
    <div class="top-frame">
      <div class="inner"><div class="top-title">タイプ</div> <div class="top-contents">{{arts.type}}</div></div>
      <div class="inner"><div class="top-title">間合い</div> <div class="top-contents">{{arts.interval}}</div></div>
      <div class="inner"><div class="top-title">コスト</div> <div class="top-contents">{{arts.cost}}</div></div>
    </div> 
    <div class="top-frame">
      <div class="inner"><div class="top-title">指定特技</div> <div class="top-contents" style="text-align: left;">{{arts.designatedSkill}}</div></div>
    </div> 
    <div class="frame bottom-frame">{{arts.description}}</div>

  </div>
</template>

<script>
export default {
  name: 'NinjaArts',
  props: ['arts'],
  data: function () {
    return {
      checked: false
    }
  },
  methods: {
    toggle: toggle
  },
  created () {
    if (this.$parent.select && this.$parent.select.includes(this.arts.id)) {
      this.checked = true
    }
  }
}

function toggle (id, checked) {
  this.checked = !this.checked
  this.$emit('toggle', id, !checked)
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.frame {
  border: black solid 1px;
}
.outer {
  /* あとで消す */
  max-width: 380px;
  /* あとで消す */
  height: 380px;
  padding: 2px;
  box-sizing: border-box;
  display: flex;
  flex-direction: column;
}

/* ヘッダー */
.arts-title {
  background: black;
  color: white;
  padding: 2px;
  display: flex;
  flex-direction: row;
  align-items: center;
}
.arts-name {
  font-size: 1.4rem;
}
.arts-name-kana {
  margin-left: 3px;
  font-size: 0.8rem;
}

/* 上部 */
.top-frame {
  border-left: black solid 1px;
  display: flex;
  overflow: hidden;
  flex-direction: row;
}
.top-frame > .inner {
  border-bottom: black solid 1px;
  border-right: black solid 1px;
  padding: 2px 4px;
  display: flex;
  flex-direction: row;
  align-items: center;
  font-size: 0.8rem;
}
.top-frame > .inner:nth-last-child(1) {
  flex-grow: 1;
}
.top-title {
  background: black;
  color: white;
  padding: 2px 6px;
  flex-grow: 0;
}
.top-contents {
  padding-left: 4px;
  padding-right: 4px;
  flex-grow: 1;
  text-align: center;
}
.arts-checkbox {
  width: 1.3rem;
  height: 1.3rem;
}

/* 下部 */
.bottom-frame {
  margin: 2px 0 0 0;
  flex-grow: 1;
  overflow: overlay;
  max-height: 312px;
  padding: 2px;
}

h1,
h2 {
  font-weight: normal;
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
