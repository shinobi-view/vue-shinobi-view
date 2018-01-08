<template>
  <div class="main">
    <div class="arts"><ninja-arts :arts="arts" @toggle="toggleArts" /></div>
    <div class="arts"><ninja-arts :arts="arts" @toggle="toggleArts" /></div>
    <div class="arts"><ninja-arts :arts="arts" @toggle="toggleArts" /></div>
    <div class="arts"><ninja-arts :arts="arts" @toggle="toggleArts" /></div>
    <div class="arts"><ninja-arts :arts="arts" @toggle="toggleArts" /></div>
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
  },
  methods: {
    toggleArts: function (checked) {
      console.log(checked)
    }
  }
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
