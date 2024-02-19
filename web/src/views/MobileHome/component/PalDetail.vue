<script setup>
import skillDescMap from "@/assets/skillDesc.json";
import { useI18n } from "vue-i18n";
import { computed } from "vue";

const { t, locale } = useI18n();

const props = defineProps(["palDetail"]);
const palDetail = computed(() => props.palDetail);

const getPalAvatar = (name) => {
  return new URL(`../../../assets/pal/${name}.png`, import.meta.url).href;
};
const getUnknowPalAvatar = () => {
  return new URL("@/assets/pal/Unknown.png", import.meta.url).href;
};

const displayHP = (hp, max_hp) => {
  return (hp / 1000).toFixed(0) + "/" + (max_hp / 1000).toFixed(0);
};

const percentageHP = (hp, max_hp) => {
  if (max_hp === 0) {
    return 0;
  }
  return ((hp / max_hp) * 100).toFixed(2);
};
</script>

<template>
  <div class="pal-detail">
    <n-space class="mb-2" justify="center">
      <n-avatar
        :size="64"
        :src="getPalAvatar(palDetail.type)"
        :fallback-src="getUnknowPalAvatar()"
      ></n-avatar>
    </n-space>
    <n-space class="mb-2" justify="center">
      <n-tag v-if="palDetail.is_boss" type="success" round>Boss</n-tag>
      <n-tag v-else-if="palDetail.is_lucky" type="warning" round>{{
        $t("pal.lucky")
      }}</n-tag>
      <n-tag v-else-if="palDetail.is_tower" type="error" round>{{
        $t("pal.tower")
      }}</n-tag>
    </n-space>
    <n-space vertical>
      <n-progress
        type="line"
        status="error"
        indicator-placement="inside"
        :percentage="percentageHP(palDetail.hp, palDetail.max_hp)"
        :height="24"
        :border-radius="4"
        :fill-border-radius="0"
        >HP: {{ displayHP(palDetail.hp, palDetail.max_hp) }}</n-progress
      >
      <n-grid cols="4">
        <n-gi>
          <n-statistic :label="$t('pal.ranged')" :value="palDetail.ranged" />
        </n-gi>
        <n-gi>
          <n-statistic :label="$t('pal.defense')" :value="palDetail.defense" />
        </n-gi>
        <n-gi>
          <n-statistic :label="$t('pal.melee')" :value="palDetail.melee" />
        </n-gi>
        <n-gi>
          <n-statistic :label="$t('pal.rank')" :value="palDetail.rank" />
        </n-gi>
      </n-grid>
    </n-space>
    <n-space vertical>
      <div v-for="skill in palDetail.skills" :key="skill">
        <n-tag type="warning">{{ skill }}</n-tag>
        :
        {{
          skillDescMap[locale][skill] ? skillDescMap[locale][skill] : "Unknown"
        }}
      </div>
    </n-space>
  </div>
</template>