<template>
  <div class="calendar">
    <header>
      <button @click="prevMonth">‹</button>
      <span>{{ monthNames[month] }} {{ year }}</span>
      <button @click="nextMonth">›</button>
    </header>
    <div class="days">
      <div class="day" v-for="(day, index) in days" :key="index">{{ day }}</div>
      <div 
        class="date" 
        v-for="(date, index) in getDatesInMonth()" 
        :key="index" 
        :class="{ today: isToday(date), selected: isSelected(date) }"
        @click="toggleDate(date)"
      >
        {{ date.getDate() }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Calendar',
  data() {
    const today = new Date();
    return {
      currentDate: today,
      selectedDate: null,
      month: today.getMonth(),
      year: today.getFullYear(),
      monthNames: ["一月", "二月", "三月", "四月", "五月", "六月", "七月", "八月", "九月", "十月", "十一月", "十二月"]
    }
  },
  computed: {
    days() {
      return ["日", "一", "二", "三", "四", "五", "六"];
    }
  },
  methods: {
    getDatesInMonth() {
      const dates = [];
      const firstDay = new Date(this.year, this.month, 1);
      const lastDay = new Date(this.year, this.month + 1, 0);
      for (let i = 1; i <= lastDay.getDate(); i++) {
        dates.push(new Date(this.year, this.month, i));
      }
      return dates;
    },
    prevMonth() {
      if (this.month === 0) {
        this.month = 11;
        this.year--;
      } else {
        this.month--;
      }
    },
    nextMonth() {
      if (this.month === 11) {
        this.month = 0;
        this.year++;
      } else {
        this.month++;
      }
    },
    toggleDate(date) {
      if (this.selectedDate && this.isSelected(date)) {
        this.selectedDate = null;
      } else {
        this.selectedDate = date;
      }
    },
    isToday(date) {
      const today = new Date();
      return today.getDate() === date.getDate() &&
             today.getMonth() === date.getMonth() &&
             today.getFullYear() === date.getFullYear();
    },
    isSelected(date) {
      if (!this.selectedDate) return false;
      return this.selectedDate.getDate() === date.getDate() &&
             this.selectedDate.getMonth() === date.getMonth() &&
             this.selectedDate.getFullYear() === date.getFullYear();
    }
  }
}
</script>

<style scoped>
.calendar {
  width: 100%;
  background-color: white; /* 设置背景为白色 */
  border: 1px solid #ccc; /* 边框颜色 */
  border-radius: 8px; /* 圆角 */
  padding: 10px; /* 内边距 */
  text-align: center; /* 文本居中 */
}

header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 10px;
}

button {
  background-color: #007bff; /* 按钮背景颜色 */
  color: white; /* 字体颜色 */
  border: none; /* 去除边框 */
  border-radius: 4px; /* 圆角 */
  padding: 10px 20px; /* 内边距 */
  font-size: 18px; /* 字体大小 */
  cursor: pointer; /* 鼠标悬浮时显示为可点击 */
  transition: background-color 0.3s; /* 添加过渡效果 */
}

button:hover {
  background-color: #0056b3; /* 悬停时的颜色 */
}

.days {
  display: grid;
  grid-template-columns: repeat(7, 1fr); /* 七列 */
}

.day, .date {
  padding: 10px;
  cursor: pointer;
  font-weight: bold; /* 加粗字体 */
  color: black; /* 字体颜色改为黑色 */
  height: 50px; /* 给日期固定高度 */
  width: 50px; /* 给日期固定宽度 */
  display: flex; /* 使用 flexbox 布局来居中内容 */
  align-items: center; /* 内容垂直居中 */
  justify-content: center; /* 内容水平居中 */
}

.today {
  background-color: #a0a0a0; /* 更醒目的灰色标识 */
  color: white; /* 字体颜色为白色 */
}

.selected {
  background-color: blue; /* 被选中日期的蓝色标识 */
  color: white; /* 字体颜色改为白色 */
}
</style>