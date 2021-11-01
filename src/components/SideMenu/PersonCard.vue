<template>
  <div class="person">
    <div class="person__photo">
      <img :src="person.picture" alt="photo" />
    </div>
    <div class="person__info">
      <div class="person__info-name">
        <b>{{ person.name }} ({{ person.age }})</b>
      </div>

      <div class="person__info-email">
        <img
          src="../../assets/icons/outline_mail_black_18dp.png"
          class="person__info-icon"
        />
        <span>{{ person.email }}</span>
      </div>
      <div class="person__info-regdate">
        <img
          src="../../assets/icons/outline_calendar_today_black_18dp.png"
          class="person__info-icon"
        />
        <span>{{ formatedDate }}</span>
      </div>
      <div class="person__info-about">
        <img src="../../assets/icons/outline_account_circle_black_18dp.png" />
        {{ person.about }}
      </div>
    </div>
  </div>
</template>

<script>
import { format } from "date-fns";
export default {
  props: {
    person: {
      type: Object,
      default: null,
    },
  },
  computed: {
    formatedDate() {
      return format(new Date(this.person.registered), "MM-dd-yyyy");
    },
  },
};
</script>

<style scoped>
.person {
  display: grid;
  grid-template-columns: 50px 1fr;
  grid-gap: 8px;
}

.person__photo img {
  height: 50px;
  width: 50px;
  border-radius: 50%;
}

.person__info {
  display: grid;
  grid-gap: 8px;
}

.person__info-name {
  margin-bottom: 10px;
  color: #2c5be8;
}

.person__info-email {
  display: flex;
  align-items: center;
  color: #354055;
}

.person__info-icon {
  margin-right: 5px;
}

.person__info-regdate {
  color: #a0a4a7;
  display: flex;
  align-items: center;
}
</style>
