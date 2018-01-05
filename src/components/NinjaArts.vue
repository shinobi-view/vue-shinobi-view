<template>
  <div class="frame outer">

    <div class="arts-title"><div class="arts-name">{{arts.name}}</div> <div class="arts-nameKana">{{arts.nameKana}}</div></div>
    <div class="upper-frame">
      <div class="inner" style="flex-basis: 40%;"><div class="upper-title">タイプ</div> <div class="upper-contents">{{arts.type}}</div></div>
      <div class="inner" style="flex-basis: 30%;"><div class="upper-title">間合い</div> <div class="upper-contents">{{arts.interval}}</div></div>
      <div class="inner" style="flex-basis: 30%;"><div class="upper-title">コスト</div> <div class="upper-contents">{{arts.cost}}</div></div>
    </div> 
    <div class="upper-frame">
      <div class="inner" style="flex-basis: 100%;"><div class="upper-title">指定特技</div> <div class="upper-contents" style="text-align: left;">{{arts.designatedSkill}}</div></div>
    </div> 

  </div>
</template>

<script>
export default {
  name: 'NinjaArts',
  props: ['arts']
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.frame {
  border: black solid 1px;
}
.outer {
  /* あとで消す */
  width: 400px;
  /* あとで消す */
  height: 400px;
  padding: 2px;
  box-sizing: border-box;
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
  font-size: 1.5rem;
}
.arts-nameKana {
  margin-left: 3px;
  font-size: 0.8rem;
}

/* 上部 */
.upper-frame {
  border-left: black solid 1px;

  display: flex;
  flex-direction: row;
}
.upper-frame > .inner {
  border-bottom: black solid 1px;
  border-right: black solid 1px;
  padding: 4px 8px;
  display: flex;
  flex-direction: row;
  align-items: center;
  font-size: 1rem;
  white-space: nowrap;
}
.upper-title {
  background: black;
  color: white;
  padding: 2px 6px;
  flex-grow: 0;
}
.upper-contents {
  padding-left: 4px;
  padding-right: 4px;
  flex-grow: 1;
  text-align: center;
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
