<script setup lang="ts">
import { ref } from 'vue';

const interval = 10000;
const elevators = ref([
  {
    id: 0, 
    isMoving: false, 
    floors: [
      { isWaiting: false, isActive: false, isOccupied: false, isUnoccupied: false, isStandBy: true },
      { isWaiting: false, isActive: false, isOccupied: false, isUnoccupied: false, isStandBy: false },
      { isWaiting: false, isActive: false, isOccupied: false, isUnoccupied: false, isStandBy: false },
      { isWaiting: false, isActive: false, isOccupied: false, isUnoccupied: false, isStandBy: false },
      { isWaiting: false, isActive: false, isOccupied: false, isUnoccupied: false, isStandBy: false },
      { isWaiting: false, isActive: false, isOccupied: false, isUnoccupied: false, isStandBy: false },
      { isWaiting: false, isActive: false, isOccupied: false, isUnoccupied: false, isStandBy: false },
      { isWaiting: false, isActive: false, isOccupied: false, isUnoccupied: false, isStandBy: false },
      { isWaiting: false, isActive: false, isOccupied: false, isUnoccupied: false, isStandBy: false },
      { isWaiting: false, isActive: false, isOccupied: false, isUnoccupied: false, isStandBy: false }
    ] 
  },
  {
    id: 1, 
    isMoving: false, 
    floors: [
      { isWaiting: false, isActive: false, isOccupied: false, isUnoccupied: false, isStandBy: true },
      { isWaiting: false, isActive: false, isOccupied: false, isUnoccupied: false, isStandBy: false },
      { isWaiting: false, isActive: false, isOccupied: false, isUnoccupied: false, isStandBy: false },
      { isWaiting: false, isActive: false, isOccupied: false, isUnoccupied: false, isStandBy: false },
      { isWaiting: false, isActive: false, isOccupied: false, isUnoccupied: false, isStandBy: false },
      { isWaiting: false, isActive: false, isOccupied: false, isUnoccupied: false, isStandBy: false },
      { isWaiting: false, isActive: false, isOccupied: false, isUnoccupied: false, isStandBy: false },
      { isWaiting: false, isActive: false, isOccupied: false, isUnoccupied: false, isStandBy: false },
      { isWaiting: false, isActive: false, isOccupied: false, isUnoccupied: false, isStandBy: false },
      { isWaiting: false, isActive: false, isOccupied: false, isUnoccupied: false, isStandBy: false }
    ] 
  },
  {
    id: 2, 
    isMoving: false, 
    floors: [
      { isWaiting: false, isActive: false, isOccupied: false, isUnoccupied: false, isStandBy: true },
      { isWaiting: false, isActive: false, isOccupied: false, isUnoccupied: false, isStandBy: false },
      { isWaiting: false, isActive: false, isOccupied: false, isUnoccupied: false, isStandBy: false },
      { isWaiting: false, isActive: false, isOccupied: false, isUnoccupied: false, isStandBy: false },
      { isWaiting: false, isActive: false, isOccupied: false, isUnoccupied: false, isStandBy: false },
      { isWaiting: false, isActive: false, isOccupied: false, isUnoccupied: false, isStandBy: false },
      { isWaiting: false, isActive: false, isOccupied: false, isUnoccupied: false, isStandBy: false },
      { isWaiting: false, isActive: false, isOccupied: false, isUnoccupied: false, isStandBy: false },
      { isWaiting: false, isActive: false, isOccupied: false, isUnoccupied: false, isStandBy: false },
      { isWaiting: false, isActive: false, isOccupied: false, isUnoccupied: false, isStandBy: false }
    ] 
  },
  {
    id: 3, 
    isMoving: false, 
    floors: [
      { isWaiting: false, isActive: false, isOccupied: false, isUnoccupied: false, isStandBy: true },
      { isWaiting: false, isActive: false, isOccupied: false, isUnoccupied: false, isStandBy: false },
      { isWaiting: false, isActive: false, isOccupied: false, isUnoccupied: false, isStandBy: false },
      { isWaiting: false, isActive: false, isOccupied: false, isUnoccupied: false, isStandBy: false },
      { isWaiting: false, isActive: false, isOccupied: false, isUnoccupied: false, isStandBy: false },
      { isWaiting: false, isActive: false, isOccupied: false, isUnoccupied: false, isStandBy: false },
      { isWaiting: false, isActive: false, isOccupied: false, isUnoccupied: false, isStandBy: false },
      { isWaiting: false, isActive: false, isOccupied: false, isUnoccupied: false, isStandBy: false },
      { isWaiting: false, isActive: false, isOccupied: false, isUnoccupied: false, isStandBy: false },
      { isWaiting: false, isActive: false, isOccupied: false, isUnoccupied: false, isStandBy: false }
    ] 
  }
]);
const elevatorLogs: any[] = ref([]);
let activeElevatorLogs = ref(0);

setInterval(() => {
  let randomStart = Math.floor(Math.random() * 10);
  let randomEnd = Math.floor(Math.random() * 10);
  let availableElevators = elevators.value.filter(e => e.isMoving == false);
  let availableElevatorsIds = availableElevators.map(e => e.id);
  let randomElevator = availableElevatorsIds[Math.floor(Math.random() * availableElevatorsIds.length)];

  if (availableElevatorsIds.length > 0 && randomStart !== randomEnd) {
    elevators.value[randomElevator].floors[randomStart].isWaiting = true;
    elevator(randomElevator, randomStart, randomEnd);
  }
}, interval);

const elevator = (elevator: number, start: number, end: number) => {
  log('waiting', elevator, start);

  elevators.value[elevator].isMoving = true;
  const currentFloor = elevators.value[elevator].floors.findIndex(f => f.isStandBy);
  const unoccupiedFloor = elevators.value[elevator].floors.findIndex(f => f.isUnoccupied);
  if (elevators.value[elevator].floors[unoccupiedFloor]) {
    elevators.value[elevator].floors[unoccupiedFloor].isUnoccupied = false;
  }

  if (currentFloor != start) {
    moveToStart(elevator, currentFloor, start)
      .then(() => {
          if (start < end) {
            up(elevator, start, end, true).then(currentFloor => setupUnoccupiedFloor(elevator, currentFloor));
          } else {
            down(elevator, start, end, true).then(currentFloor => setupUnoccupiedFloor(elevator, currentFloor));
          }
      });
  } else {
    if (start < end) {
      return up(elevator, start, end, true).then(currentFloor => setupUnoccupiedFloor(elevator, currentFloor));
    } else {
      return down(elevator, start, end, true).then(currentFloor => setupUnoccupiedFloor(elevator, currentFloor));
    }
  }
}

const moveToStart = (elevator: number, currentFloor: number, start: number) => {
  return new Promise((resolve) => {
    if (currentFloor < start) {
      up(elevator, currentFloor, start, false).then(resolve);
    } else {
      down(elevator, currentFloor, start, false).then(resolve);
    }
  });
}

const up = (elevator: number, start: number, end: number, isOccupied: boolean) => {
  return new Promise((resolve) => {
    let loopIndex = 0;
    let currentFloor = start;

    const i = setInterval(() => {
      setupOccupiedFloor(elevator, currentFloor, isOccupied, loopIndex, end);

      if (elevators.value[elevator].floors[currentFloor - 1]) {
        elevators.value[elevator].floors[currentFloor - 1].isActive = false;
        elevators.value[elevator].floors[currentFloor - 1].isOccupied = false;
      }
      
      if (currentFloor === end) {
        if (isOccupied) {
          log('arrived', elevator, currentFloor);
        }
        clearInterval(i);
        resolve(currentFloor);
      }

      currentFloor++;
      loopIndex++;
    }, interval);
  });
}

const down = (elevator: number, start: number, end: number, isOccupied: boolean) => {
  return new Promise((resolve) => {
    let loopIndex = 0;
    let currentFloor = start;
    
    const i = setInterval(() => {
      setupOccupiedFloor(elevator, currentFloor, isOccupied, loopIndex, end);
      
      if (elevators.value[elevator].floors[currentFloor + 1]) {
        elevators.value[elevator].floors[currentFloor + 1].isActive = false;
        elevators.value[elevator].floors[currentFloor + 1].isOccupied = false;
      }
      
      if (currentFloor === end) {
        if (isOccupied) {
          log('arrived', elevator, currentFloor);
        }
        clearInterval(i);
        resolve(currentFloor);
      }

      currentFloor--;
      loopIndex++;
    }, interval);
  });
}

const setupUnoccupiedFloor = (elevator: number, currentFloor: number) => {
  setTimeout(() => {
    elevators.value[elevator].floors[currentFloor].isStandBy = true;
    elevators.value[elevator].floors[currentFloor].isActive = false;
    elevators.value[elevator].floors[currentFloor].isOccupied = false;
    elevators.value[elevator].floors[currentFloor].isUnoccupied = true;
    elevators.value[elevator].isMoving = false;
    log('left', elevator, currentFloor);
  }, interval);
}

const setupOccupiedFloor = (elevator: number, currentFloor: number, isOccupied: boolean, loopIndex: number, end: number) => {
  elevators.value[elevator].floors[currentFloor].isStandBy = false;
  elevators.value[elevator].floors[currentFloor].isActive = true;
  if (isOccupied) {
    elevators.value[elevator].floors[currentFloor].isWaiting = false;
    elevators.value[elevator].floors[currentFloor].isOccupied = true;
    if (loopIndex == 0) {
      log('picked', elevator, currentFloor);
    }
  }
  if(!isOccupied && loopIndex == 0) {
    log('picking', elevator, end);
  }
}

const manageLogs = (id: number) => {
  const elevatorLogsById = elevatorLogs.value.filter(e => e.id === id);
  const index = elevatorLogs.value.findIndex(e => e.id === id && e.event === elevatorLogsById[0].event);
  if (elevatorLogsById.length == 5) {
    elevatorLogs.value.splice(index, 1);
  }
}

const log = (event: string, elevator: number, floor: number) => {
  let id = elevator+1;
  let isVisible = (id == activeElevatorLogs.value || activeElevatorLogs.value == 0) ? true : false;
  let message = "";
  if (event == 'waiting') {
    message = `A passenger is waiting in ${floor+1}F`;
  }
  if (event == 'picking') {
    message = `Elevator ${id} is picking a passenger in ${floor+1}F`;
  }
  if (event == 'picked') {
    message = `Elevator ${id} picked a passenger in ${floor+1}F`
  }
  if (event == 'arrived') {
    message = "Passenger reached his/her destination";
  }
  if (event == 'left') {
    message = `A passenger left the Elevator ${id} in ${floor+1}F`
  }
  
  manageLogs(id);
  elevatorLogs.value.push({ id, event, class: `elevator-${id}`, message, isVisible });
}

const toggleElevatorLogs = (id: number) => {
  activeElevatorLogs.value = id;
  elevatorLogs.value.forEach(eLog => {
    if (eLog.id === id || id === 0) {
      eLog.isVisible = true;
    } else {
      eLog.isVisible = false;
    }
  });
}
</script>

<template>
  <div class="building">
    <div class="elevator">
        <div class="elevator-col" v-for="elevator in elevators">
          <div class="elevator-row" v-for="floor in elevator.floors" :class="{ waiting: floor.isWaiting, occupied: floor.isOccupied, 'stand-by': floor.isStandBy, active: floor.isActive, unoccupied: floor.isUnoccupied }"></div>
        </div>
    </div>
    <div class="elevator-logs">
      <div class="elevator-logs-action">
        <button class="elevator-logs-action-item" @click="toggleElevatorLogs(0)"></button>
        <button class="elevator-logs-action-item" @click="toggleElevatorLogs(1)"></button>
        <button class="elevator-logs-action-item" @click="toggleElevatorLogs(2)"></button>
        <button class="elevator-logs-action-item" @click="toggleElevatorLogs(3)"></button>
        <button class="elevator-logs-action-item" @click="toggleElevatorLogs(4)"></button>
      </div>
      <ul class="elevator-logs-list">
        <li class="elevator-logs-list-item" v-for="eLog in elevatorLogs" :class="[ eLog.isVisible ? 'show' : 'hide' ]"><span :class="eLog.class">{{ eLog.message }}</span></li>
      </ul>
    </div>
  </div>
  
</template>

<style scoped>
.building {
  color: #464646;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  gap: 5px;
  .elevator {
    display: flex;
    flex-direction: row;
    justify-content: center;
    gap: 50px;
    padding: 40px 20px 20px 20px;
  }
  .elevator-col {
    display: flex;
    flex-direction: column-reverse;
    gap: 10px;
    &:nth-child(1) .occupied {
      background-color: #ffa4e4;
    }
    &:nth-child(2) .occupied {
      background-color: #a4b2ff;
    }
    &:nth-child(3) .occupied {
      background-color: #a4deff;
    }
    &:nth-child(4) .occupied {
      background-color: #a4ffeb;
    }
  }
  .elevator-row {
    width: 30px;
    height: 40px;
    background-color: #d6d6d6;
    position: relative;
    &::before {
      content: "";
      display: none;
      width: 12px;
      height: 12px;
      border-radius: 12px;
      position: absolute;
      right: 0;
      bottom: 16px;
      left: 0;
      margin: auto;
      background-color: #707070;
    }
    &::after {
      content: "";
      display: none;
      width: 10px;
      height: 12px;
      position: absolute;
      right: 0;
      bottom: 2px;
      left: 0;
      margin: auto;
      border-top-left-radius: 20px;
      border-top-right-radius: 20px;
      background-color: #707070;
    }
    &.waiting::before,
    &.waiting::after,
    &.occupied::before,
    &.occupied::after,
    &.unoccupied::before,
    &.unoccupied::after {
      display: block;
      background-color: #707070;
    }
    &.waiting::before {
      left: -50px;
    }
    &.waiting::after {
      left: -50px;
    }
    &.occupied::before,
    &.occupied::after {
      left: 0;
    }
    &.unoccupied::before {
      right: -50px;
    }
    &.unoccupied::after {
      right: -50px;
    }
    &.stand-by {
      background-color: #a8a8a8;
    }
    &.active {
      background-color: #b8ffb8;
    }
  }
  .elevator-logs {
    text-align: center;
    padding: 0 10px;
  }
  .elevator-logs-action {
    margin-bottom: 10px;
    display: flex;
    flex-direction: row;
    gap: 5px;
    .elevator-logs-action-item {
      width: 100%;
      height: 20px;
      border: none;
      cursor: pointer;
      &:hover {
        position: relative;
        top: -2px;
      }
      &:active {
        top: unset;
      }
      &:nth-child(1) {
        background-color: #d3d3d3;
      }
      &:nth-child(2) {
        background-color: #ffa4e4;
      }
      &:nth-child(3) {
        background-color: #a4b2ff;
      }
      &:nth-child(4) {
        background-color: #a4deff;
      }
      &:nth-child(5) {
        background-color: #a4ffeb;
      }
    }
  }
  .elevator-logs-list {
    height: 100%;
    overflow: auto;
    display: flex;
    gap: 3px;
    flex-direction: column;
    font-size: 12px;
    padding: 0;
  }
  .elevator-logs-list-item {
    &.hide {
      display: none;
    }
    span {
      padding: 3px 5px;
      border-radius: 5px;
      display: block;
      &.elevator-1 {
        background-color: #ffa4e4;
      }
      &.elevator-2 {
        background-color: #a4b2ff;
      }
      &.elevator-3 {
        background-color: #a4deff;
      }
      &.elevator-4 {
        background-color: #a4ffeb;
      }
    }
  }
}
@media only screen and (min-width: 768px) {
  .building {
    gap: 30px !important;
  }
  .elevator {
    padding-top: 50px !important;
  }
  .elevator-row {
    width: 50px !important;
    height: 60px !important;
    &::before {
      width: 20px !important;
      height: 20px !important;
      border-radius: 20px !important;
      bottom: 19px !important;
    }
    &::after {
      width: 13px !important;
      height: 15px !important;
    }
    &.waiting::before {
      left: -80px !important;
    }
    &.waiting::after {
      left: -80px !important;
    }
    &.unoccupied::before {
      right: -80px !important;
    }
    &.unoccupied::after {
      right: -80px !important;
    }
  }
  .elevator-logs {
    padding: 0 100px !important;
  } 
}
@media only screen and (min-width: 992px) {
  .building {
    flex-direction: row;
    justify-content: normal;
  }
  .elevator {
    width: 60%;
    gap: 70px !important;
  }
  .elevator-logs {
    padding: 40px 10px 0 10px !important;
    width: 250px;
  }
  .elevator-row {
    width: 50px !important;
    height: 60px !important;
  }
}
</style>
