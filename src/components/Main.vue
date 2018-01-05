<template>
  <div class="main">
    <ninja-arts :arts="arts"/>
  </div>
</template>

<script>
import ninjaArts from '@/components/NinjaArts'

export default {
  components: {
    'ninja-arts': ninjaArts
  },
  name: 'Main',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      arts: {
        name: '@忍法学',
        nameKana: 'にんぽうがく',
        type: 'サポート忍法',
        interval: 'なし',
        cost: 'なし',
        designatedSkill: '針術、身体操術、隠蔽術、罠術、地の利、瞳術',
        description: `装備忍法の【博識】（p84）の効果で得る奥義破り判定への修正が、プラス2からプラス4になる。`,
        sceneDescription: '古今東西の忍法について調べ尽くしている。'
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
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
