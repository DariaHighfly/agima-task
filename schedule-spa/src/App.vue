<template>
  <div class="main">
    <div class="schedule">
      <div class="time">
        <div class="transfer">
          <img class="transfer__img" src="./assets/agima.jpeg" alt="drag-image"/>
        </div>
        <div class="timetable">
          <table class="timetable__table">
            <tr class="timetable__title">
              <th class="timetable__title_item">Время</th>
              <th class="timetable__title_item">Событие</th>
              <th></th>
            </tr>
            <tr class="timetable__list"
                v-for="item in scheduleList[currentDay]" :key="item.id">
              <td>
                <textarea v-model="item.time" class="timetable__list_time"></textarea>
              </td>
              <td>
                <textarea v-model="item.event" class="timetable__list_name"></textarea>
              </td>
              <td>
                <img class="timetable__list_delete"
                     src="./assets/agima.jpeg"
                     alt="delete-image"
                     v-on:click="deleteEvent(item.id)"
                />
              </td>
            </tr>
            <tr class="timetable_add">
              <td><input v-model="newTime" class="timetable__input_time"></td>
              <td><input v-model="newEvent" class="timetable__input_name"></td>
              <td>
                <img class="timetable__list_add"
                     src="./assets/agima.jpeg"
                     alt="delete-image"
                     v-on:click="addEvent"
                />
              </td>
            </tr>
          </table>
        </div>
      </div>
      <div class="week-day">
        <ul class="week-day__list">
          <li v-for="day in daysOfWeek"
              :key="day.key"
              class="week-day__list_item"
              :class="{'selected-day ' : day.key === currentDay}"
              v-on:click="changeDay(day.key)">{{day.weekDay}}</li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      currentDay: ((new Date()).getDay() === 0) ? 6 : (new Date()).getDay() - 1,
      newTime: "",
      newEvent: "",
      maxId: [4, 1, 0, 0, 0, 1, 0],
      daysOfWeek: [
        {
          key: 0,
          weekDay: "Пн"
        },
        {
          key: 1,
          weekDay: "Вт"
        },
        {
          key: 2,
          weekDay: "Ср"
        },
        {
          key: 3,
          weekDay: "Чт"
        },
        {
          key: 4,
          weekDay: "Пт"
        },
        {
          key: 5,
          weekDay: "Сб"
        },
        {
          key: 6,
          weekDay: "Вс"
        }
      ],
      scheduleList: [
              [
                {
                  id: 0,
                  time: "10:00",
                  event: "Обед"
                },
                {
                  id: 1,
                  time: "11:00",
                  event: "Обед"
                },
                {
                  id: 2,
                  time: "12:00",
                  event: "Обед"
                },
                {
                  id: 3,
                  time: "13:00",
                  event: "Обед"
                },
              ],
              [
                {
                  id: 0,
                  time: "10:00",
                  event: "Обед"
                },
              ],
              [],
              [],
              [
                {
                  id: 0,
                  time: "15:00",
                  event: "Залить код"
                },
                {
                  id: 1,
                  time: "19:00",
                  event: "Сдать тестовое задание, чтобы попасть на стажировку"
                }
              ],
              [
                {
                  id: 0,
                  time: "20:00",
                  event: "Ужин"
                },
              ],
              []
      ]
    }
  },
  methods: {
    addEvent() {
      if (this.newTime !== "" && this.newEvent !== "") {
        this.maxId[this.currentDay] += 1;
        this.scheduleList[this.currentDay].push(
                {
                  id: this.maxId[this.currentDay],
                  time: this.newTime,
                  event: this.newEvent
                }
        );
      }
      this.newTime = "";
      this.newEvent = "";
    },
    deleteEvent(id) {
      let index = this.scheduleList[this.currentDay].findIndex((elem) => elem.id === id);
      this.scheduleList[this.currentDay].splice(index, 1);
    },
    changeDay(key) {
      this.currentDay = key;
    }
  },
  mounted() {
    if (localStorage.newTime) {
      this.newTime = localStorage.newTime;
    }
  },
}
</script>

<style>
  @font-face {
    font-family: "Gilroy";
    src: url('assets/Gilroy-Regular.eot');
  }
  body {
    padding: 0;
    margin: 0;
  }
  .main {
    font-family: "Gilroy", sans-serif;
    display: flex;
    justify-content: center;
    align-items: center;
    background-color: #f5f7f7;
    height: 100vh;
  }
  .schedule {
    display: flex;
    flex-direction: row;
    justify-content: center;
    align-items: flex-start;
    background-color: #ffffff;
    width: 500px;
    height: 600px;
    padding:50px;
    /*border: 3px solid #01ac6a;*/
    border-radius: 5px;
    box-shadow: 10px 10px 14px -3px rgba(0,0,0,0.05);
  }
  .transfer {
    display: flex;
    justify-content: flex-end;
    margin: 10px;
  }
  .transfer__img {
    width: 25px;
  }
  .transfer__img:hover {
    cursor: move;
  }
  .time {
    display: flex;
    flex-direction:column;
    margin-right: 40px;
    box-shadow: 4px 4px 13px 10px rgba(0,0,0,0.03);
    border-radius: 5px;
    width: 100%;
    min-height: 80%;
  }
  .timetable {
    display: flex;
    justify-content: flex-end;
    padding: 30px 10px 0 80px;
  }
  table {
    table-layout: auto;
    width: 100%;
  }
  table th, table td {
    text-align: left;
    vertical-align: top;
  }
  table th {
    font-size: 16px;
    font-weight: normal;
    padding-bottom: 20px;
  }
  .timetable__list_time, .timetable__list_name {
    outline: none;
    border: none;
    resize: none;
    font-family: "Gilroy", sans-serif;
    font-size: 14px;
    font-weight: normal;
  }
  .timetable__list_time {
    width: 50px;
    font-size: 16px;
    font-weight: bold;
    color: #01ac6a;
  }
  .timetable__list_name {
    width: 160px;
    min-height: 60px;
  }
  .timetable__list_delete, .timetable__list_add {
    width: 20px;
    height: 20px;
    margin-left: 40px;
    padding: 0;
  }
  .timetable__list_add {
    width: 20px;
    height: 20px;
    margin: 30px 0 0 40px;
  }
  .timetable__input_time, .timetable__input_name {
    border: 2px solid #f0f2f2;
    border-radius: 4px;
    margin: 30px 0 0 0;
    resize: none;
    outline: none;
  }
  .timetable__input_time {
    width: 60px;
    height: 30px;
  }
  .timetable__input_name {
    width: 160px;
    height: 30px;
  }
  .week-day {
    background-color: #f5f7f7;
  }
  .week-day__list {
    display: flex;
    flex-direction:column;
    justify-content: center;
    align-items: center;
    list-style-type: none;
    padding: 0;
    margin: 0;
    font-size: 16px;
  }
  .week-day__list_item {
    padding: 10px 30px 10px 30px;
  }
  .week-day__list_item:hover {
    background-color: #01ac6a;
    cursor: pointer;
  }
  .selected-day {
    background-color: #01ac6a;
  }
</style>
