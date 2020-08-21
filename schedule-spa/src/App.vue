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
              <td class="timetable__list_time">{{item.time}}</td>
              <td class="timetable__list_name">{{item.event}}</td>
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
          weekDay: "пн"
        },
        {
          key: 1,
          weekDay: "вт"
        },
        {
          key: 2,
          weekDay: "ср"
        },
        {
          key: 3,
          weekDay: "чт"
        },
        {
          key: 4,
          weekDay: "пт"
        },
        {
          key: 5,
          weekDay: "сб"
        },
        {
          key: 6,
          weekDay: "вс"
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
                  event: "Сдать тестовое задание"
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
  }
}
</script>

<style>
  body {
    padding: 0;
    margin: 0;
  }
  .main {
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
    table-layout: fixed;
    width: 100%;
  }
  .timetable__title {
  }
  .timetable__title_item {
  }
  .timetable__title_item:last-child {
  }
  .timetable__list, .timetable_add {
  }
  .timetable__title_item, .timetable__list_time, .timetable__list_name {
    text-align: left;
  }
  .timetable__list_delete, .timetable__list_add {
    width: 20px;
    height: 20px;
    margin-left: 20px;
    padding: 0;
  }
  .timetable__list_add {
    margin: 30px 0 0 20px;
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
    width: 90px;
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
    font-size: 18px;
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
