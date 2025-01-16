<script setup lang="ts">
import { ref } from 'vue';

const interval = 3000;
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
    let currentFloor = start;
    const i = setInterval(() => {
      setupOccupiedFloor(elevator, currentFloor, isOccupied);

      if (elevators.value[elevator].floors[currentFloor - 1]) {
        elevators.value[elevator].floors[currentFloor - 1].isActive = false;
        elevators.value[elevator].floors[currentFloor - 1].isOccupied = false;
      }
      
      if (currentFloor === end) {
        clearInterval(i);
        resolve(currentFloor);
      }

      currentFloor++;
    }, interval);
  });
}

const down = (elevator: number, start: number, end: number, isOccupied: boolean) => {
  
  return new Promise((resolve) => {
    let currentFloor = start;
    const i = setInterval(() => {
      setupOccupiedFloor(elevator, currentFloor, isOccupied);
      
      if (elevators.value[elevator].floors[currentFloor + 1]) {
        elevators.value[elevator].floors[currentFloor + 1].isActive = false;
        elevators.value[elevator].floors[currentFloor + 1].isOccupied = false;
      }
      
      if (currentFloor === end) {
        clearInterval(i);
        resolve(currentFloor);
      }

      currentFloor--;
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
  }, interval);
}

const setupOccupiedFloor = (elevator: number, currentFloor: number, isOccupied: boolean) => {
  elevators.value[elevator].floors[currentFloor].isStandBy = false;
  elevators.value[elevator].floors[currentFloor].isActive = true;
  if (isOccupied) {
    elevators.value[elevator].floors[currentFloor].isWaiting = false;
    elevators.value[elevator].floors[currentFloor].isOccupied = true;
  }
}
</script>

<template>
  <div class="building">
    <div class="elevators" v-for="elevator in elevators">
      <div class="elevator" v-for="floor in elevator.floors" :class="{ waiting: floor.isWaiting, occupied: floor.isOccupied, 'stand-by': floor.isStandBy, active: floor.isActive, unoccupied: floor.isUnoccupied }"></div>
    </div>
  </div>
</template>

<style scoped>
  .building {
    display: flex;
    flex-direction: row;
    gap: 20px 80px;
    justify-content: center;
    padding: 50px;
    position: relative;
  }

  .elevators {
    display: flex;
    flex-direction: column-reverse;
    gap: 20px;
  }

  .elevator {
    width: 50px;
    height: 60px;
    background-color: #d6d6d6;
    position: relative;

    &::before {
      content: "";
      display: none;
      width: 20px;
      height: 20px;
      border-radius: 20px;
      position: absolute;
      right: 0;
      bottom: 16px;
      left: 0;
      margin: auto;
    }

    &::after {
      content: "";
      display: none;
      width: 15px;
      height: 13px;
      position: absolute;
      right: 0;
      bottom: 2px;
      left: 0;
      margin: auto;
      border-top-left-radius: 20px;
      border-top-right-radius: 20px;
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
      left: -75px;
    }
    &.waiting::after {
      left: -76px;
    }
    &.occupied::before,
    &.occupied::after {
      left: 0;
    }
    &.unoccupied::before {
      right: -75px;
    }
    &.unoccupied::after {
      right: -76px;
    }
    &.stand-by {
      background-color: #a8a8a8;
    }
    &.active {
      background-color: #b8ffb8;
    }
    &.occupied {
      background-color: #ffc8a4;
    }
  }
</style>
