<template>
  <div :class="className + (isStatic ? ' static' : '')">
    <section class="search-result-name">
      {{name(true)}}
    </section>
    <section class="search-result-location">
      <i class="fa fa-map-marker-alt fa-fw"></i>
      {{getMapNameForObj(data)}} {{getMapStaticStringForObj(data)}}
    </section>
    <section class="search-result-id">
      <i class="fas fa-hashtag fa-fw"></i> ID
      <span v-if="withPermalink"><router-link :to="{ query: { id: `${data.map_type},${data.map_name},${data.hash_id}` } }" append>{{formatObjId(data.hash_id)}}</router-link></span>
      <span v-if="!withPermalink">{{formatObjId(data.hash_id)}}</span>
    </section>
    <section class="search-result-hard-mode">
      <span style="color: #ff3915" v-if="data.hard_mode"><i class="fa fa-dungeon fa-fw"></i> Master Mode</span>
    </section>
    <section class="search-result-oho">
      <span style="color: #ff3915" v-if="data.one_hit_mode"><i class="fa fa-infinity fa-fw"></i> One-Hit Obliterator Object</span>
    </section>
    <section class="search-result-drop" v-if="!dropAsName && data.drop"><i class="fa fa-gem fa-fw"></i> {{drop()}}</section>
    <section class="search-result-equip" v-if="data.equip"><i class="fa fa-shield-alt fa-fw"></i> {{data.equip.map((x) => getName(x)).join(', ')}}</section>
    <section class="search-result-location" v-if="data.location">
      <i class="fas fa-fw fa-location-arrow" aria-hidden="true" style="color: honeydew"></i>
      {{data.location}}
    </section>
    <section class="search-result-scale" v-if="data.scale === 0">
      <i class="fas fa-fw fa-ban" style="color:tomato"></i>
      {{data.name.includes('Enemy_Giant') ? 'No enemy scaling' : 'No scaling'}}
    </section>
    <section class="search-result-rankup" v-if="isActuallyRankedUp(data)">
      <i class="fas fa-fw fa-chevron-up"></i>
      Ranked up from {{name(false)}}
    </section>
    <section class="search-result-rankup" v-if="data.disable_rankup_for_hard_mode">
      <i class="fas fa-fw fa-ban" style="color:tomato"></i>
      No rankup in Master Mode
    </section>
    <section class="search-result-rankup" v-if="data.spawns_with_lotm" title="Spawns only when Lord of the Mountain is present">
      <i class="fas fa-fw fa-horse" style="text-shadow: lightblue 0px 0px 5px; color: lightblue;"></i>
      Lord of the Mountain
    </section>
    <section class="search-result-rankup" v-if="data.field_area == 28">
      <i class="fas fa-fw fa-ban" style="color:tomato"></i>
      No scaling on Eventide Island
    </section>
    <section class="search-result-bonus" v-if="data.sharp_weapon_judge_type > 0">
      <span v-if="data.sharp_weapon_judge_type == 1"><i class="far fa-star fa-fw" style="color: deepskyblue"></i> Minimum modifier tier: Blue (random)</span>
      <span v-if="data.sharp_weapon_judge_type == 2"><i class="fas fa-star fa-fw" style="color: deepskyblue"></i> Minimum modifier tier: Blue</span>
      <span v-if="data.sharp_weapon_judge_type == 3"><i class="fas fa-star fa-fw" style="color: #ffc700"></i> Minimum modifier tier: Yellow</span>
      <span v-if="data.sharp_weapon_judge_type == 4"><i class="far fa-star fa-fw" style="color: tomato"></i> No modifier</span>
    </section>
    <section class="search-result-link" v-if="link"><i class="fa fa-link fa-fw"></i> Link type: {{link.ltype}}</section>
    <section class="search-result-yahaha" v-if="data.korok_type">
      <i class="fas fa-fw fa-leaf" style="color:lightgreen"></i>
      {{data.korok_id}} - {{ data.korok_type }}
    </section>
  </div>
</template>
<style lang="less">
.search-result {
  border-radius: 3px;
  border: 1px solid #a2a2a27a;
  background: rgba(0, 0, 0, 0.35);
  padding: 10px 10px;
  margin-bottom: 10px;
  font-size: 85%;
  transition: background 0.2s, border 0.2s;

  &:not(.static) {
    cursor: pointer;
    &:hover {
      background: rgba(0, 0, 0, 0.15);
      border: 1px solid #c2c2c27a;
    }
  }
}

.search-result-name {
  font-size: 110%;
  margin-bottom: 3px;
}
</style>
<script src="./ObjectInfo.ts"></script>
