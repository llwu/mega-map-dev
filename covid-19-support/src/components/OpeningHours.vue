<template>
  <div class="openhours" v-if="days.length > 0">
    <div class="oh-title">
      <b>{{ title }}</b>
    </div>
    <table>
      <tr v-for="(item, index) in days" :key="index" class="oh-row">
        <!-- <i class="fas" :class="icon" /> -->
        <td class="oh-name">{{ item.name }}</td>
        <td :class="item.class"><span v-html="item.val"></span></td>
      </tr>
    </table>
  </div>
</template>

<script>
import { dayFilters, seniorDayFilters, weekdayHours } from '@/constants'
export default {
  name: 'OpeningHours',
  data() {
    return {}
  },
  props: {
    title: { type: String },
    infotype: { type: String },
    icon: { type: String },
    senior: { type: Boolean },
    business: { type: Object }
  },
  computed: {
    days() {
      var myDays = []
      var cnt = 0

      if (this.senior) {
        seniorDayFilters.forEach((attr, index) => {
          var dayName = this.$t(`dayofweek.${weekdayHours[index].day}`)
          switch (this.business[attr].length) {
            case 0:
              // myDays.push({ name: dayName, val: this.$t('label.normalhours') })
              // cnt++
              break
            case 1:
              // if (this.business[attr] == '0') {
              //   myDays.push({ name: dayName, val: this.$t('label.closed') })
              // } else {
              //   // myDays.push({ name: dayName, val: this.$t('label.normalhours') })
              //   // cnt++
              // }
              break
            default:
              myDays.push({ name: dayName, val: this.business[attr].replace(/,/g, '<br/>') })
              cnt++
              break
          }
        })
      } else {
        dayFilters.forEach((attr, index) => {
          var dayName = this.$t(`dayofweek.${weekdayHours[index].day}`)
          switch (this.business[attr].length) {
            case 0:
              // myDays.push({ name: dayName, val: this.$t('label.normalhours') })
              // cnt++
              break
            case 1:
              if (this.business[attr] == 0) {
                myDays.push({ name: dayName, val: this.$t('label.closed'), class: 'openingHoursClosed' })
              } else {
                // myDays.push({ name: dayName, val: this.$t('label.normalhours') })
                // cnt++
              }
              cnt++
              break
            default:
              myDays.push({ name: dayName, val: this.business[attr].replace(/,/g, '<br/>') })
              cnt++
              break
          }
        })
      }

      if (cnt == 0) {
        return myDays.push({ name: this.$t('label.allweek'), val: this.$t('label.closed') })
      }

      return myDays
    }
  }
}
</script>

<style>
.openhours {
  margin-bottom: 16px;
}
.oh-title {
  font-size: 0.9rem;
  margin-bottom: 4px;
}
.oh-name {
  padding: 2px 20px 2px 0;
  vertical-align: top;
}
.oh-row {
  padding-bottom: 4px;
}
</style>
