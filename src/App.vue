<script setup lang="ts">
import { ref } from 'vue';

const interval = 5000;
let elevators = ref([
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
let elevatorLogs: any[] = []; 

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
  manageLogs();
  elevatorLogs.push({ class: `elevator-${elevator+1}`, message: `A passenger is waiting in ${start+1}F` });

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
          manageLogs();
          elevatorLogs.push({ class: `elevator-${elevator+1}`, message: `Passenger reached his/her destination` });
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
    manageLogs();
    elevatorLogs.push({ class: `elevator-${elevator+1}`, message: `A passenger left the Elevator ${elevator+1} in ${currentFloor+1}F` });
  }, interval);
}

const setupOccupiedFloor = (elevator: number, currentFloor: number, isOccupied: boolean, loopIndex: number, end: number) => {
  elevators.value[elevator].floors[currentFloor].isStandBy = false;
  elevators.value[elevator].floors[currentFloor].isActive = true;
  if (isOccupied) {
    elevators.value[elevator].floors[currentFloor].isWaiting = false;
    elevators.value[elevator].floors[currentFloor].isOccupied = true;
    if (loopIndex == 0) {
      manageLogs();
      elevatorLogs.push({ class: `elevator-${elevator+1}`, message: `Elevator ${elevator+1} picked a passenger in ${currentFloor+1}F` });
    }
  }
  if(!isOccupied && loopIndex == 0) {
    manageLogs();
    elevatorLogs.push({ class: `elevator-${elevator+1}`, message: `Elevator ${elevator+1} is picking a passenger in ${end+1}F` });
  }
}

const manageLogs = () => {
  if (elevatorLogs.length == 20) {
    elevatorLogs.shift();
  }
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
      <ul class="elevator-logs-list">
        <li class="elevator-logs-list-item" v-for="eLog in elevatorLogs" :class="eLog.class">{{ eLog.message }}</li>
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
    padding: 3px 5px;
    border-radius: 5px;
    &.elevator-default {
      background-color: #e4e4e4;
    }
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
