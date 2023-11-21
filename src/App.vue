<template>
  <div id="app">
    <div id="map-container">
      <img id="map" src="@/assets/map.png" alt="Map Background" />
      <img id="mario" @click="selectLevel" :src="marioImage" :style="{
        top: marioPosition.top + 'px',
        left: marioPosition.left + 'px',
      }" />
      <audio id="backgroundMusic" loop autoplay>
        <source src="@/assets/musicMarioBros.mp3" type="audio/mp3" />
      </audio>
      <div>
        <div v-if="selectedLevel">
          <button @click="backToSelection">Back to Level Selection</button>
          <component :is="selectedLevelComponent" :background="selectedLevel.background" />
        </div>
        <div v-else>
          <h1>Level Selection</h1>
          <div>
            <div @click="selectLevel(1)" :class="{ selected: selectedLevel === 1 }">
              <img src="@/assets/lvl1.png" alt="Level 1" />
            </div>
            <div @click="selectLevel(2)" :class="{ selected: selectedLevel === 2 }">
              <img src="@/assets/lvl2.png" alt="Level 2" />
            </div>
          </div>
          <div id="coordinates">
            Coordinates: {{ marioPosition.left }}, {{ marioPosition.top }}
          </div>
        </div>
      </div>

      <div id="coordinates">
        Coordinates: {{ marioPosition.left }}, {{ marioPosition.top }}
      </div>
    </div>
    <div v-if="selectedLevel" id="level-info">
      <h1>{{ selectedLevel.title }}</h1>
      <p>{{ selectedLevel.description }}</p>
      <button @click="backToSelection">Back to Level Selection</button>
    </div>
    <div v-if="selectedLevel" id="level-info" :style="{ backgroundImage: 'url(' + selectedLevel.background + ')' }">
      <h1>{{ selectedLevel.title }}</h1>
      <a v-if="selectedLevel.link" :href="selectedLevel.link" target="_blank">
        <img :src="require('@/assets/paperPxlArt.png')" alt="Paper Pxl Art" style="max-width: 50px;">
      </a>
      <p>{{ selectedLevel.description }}</p>
      <table>
        <thead>
          <tr>
            <th>Fecha</th>
            <th>Actividad</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="activity in selectedLevel.activities" :key="activity.date" class="activity-row">
            <td>{{ activity.date }}</td>
            <td>{{ activity.activity }}</td>
          </tr>
        </tbody>
      </table>

      <button @click="backToSelection">Volver al mapa</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      marioSize: 100,
      initialWindowSize: {
        width: 800,
        height: 600,
      },
      selectedLevel: null,
      level: [
        {
          title: "Level 1",
          description: "Descripcion de TDR para Fendermed",
          background: require("@/assets/lvl1.png"),
          link: "https://docs.google.com/document/d/1L9bep_QlKBWPA2dte0BrOXx8kld3o3wijRaB1E9zJEE/edit",
          activities: [
            {
              date: "2022-01-01",
              activity: "Reuniones Iniciales con Interesados",
            },
            {
              date: "2022-01-02",
              activity: "Definición de Objetivos y Alcance",
            },
            {
              date: "2022-01-03",
              activity: "Análisis de Requisitos",
            },
            {
              date: "2022-01-04",
              activity: "Investigación de Tecnologías",
            },
          ],
        },
        {
          title: "Level 2",
          description: "Descripcion de las herramientas usadas",
          background: require("@/assets/lvl2.png"),
          link: "https://docs.google.com/document/d/1UNgwC10LiXWrkK79f4J5aOiwt5whPpPwMUxLq32SihQ/edit",
          activities: [
            {
              date: "2022-01-01",
              activity: "Definición del Proyecto",
            },
            {
              date: "2022-01-02",
              activity: "Identificación de Interesados",
            },
            {
              date: "2022-01-03",
              activity: "Estudio de Viabilidad",
            },
            {
              date: "2022-01-04",
              activity: "Formación del Equipo del Proyecto",
            },
          ],
        },
        {
          title: "Level 3",
          description: "Sky High Challenge (Herramientas que se usaran)",
          background: require("@/assets/level3.png"),
          link: "https://docs.google.com/document/d/15Z26m_yu_WGgBsiHr089wSMsjZRSPVtDN2mwLHxlOs4/edit",
          activities: [
            {
              date: "1",
              activity: "YouTrack",
            },
            {
              date: "2",
              activity: "Mockups.com",
            },
            {
              date: "3",
              activity: "Draw.io",
            },
          ],
        },
        {
          title: "Level 4",
          description: "Haunted Mansion Mystery",
          background: require("@/assets/level4.png"),
          activities: [
            {
              date: "2022-01-01",
              activity: "Actividad 1",
            },
            {
              date: "2022-01-02",
              activity: "Actividad 2",
            },
          ],
        },
        {
          title: "Level 5",
          description: "Lava Lagoon Escape",
          background: require("@/assets/level5.png"),
          activities: [
            {
              date: "2022-01-01",
              activity: "Actividad 1",
            },
            {
              date: "2022-01-02",
              activity: "Actividad 2",
            },
          ],
        },
        {
          title: "Level 6",
          description: "Ice Kingdom Journey",
          background: require("@/assets/level6.png"),
          activities: [
            {
              date: "2022-01-01",
              activity: "Actividad 1",
            },
            {
              date: "2022-01-02",
              activity: "Actividad 2",
            },
          ],
        },
        {
          title: "Level 7",
          description: "Final Castle Showdown",
          background: require("@/assets/level7.png"),
          activities: [
            {
              date: "2022-01-01",
              activity: "Actividad 1",
            },
            {
              date: "2022-01-02",
              activity: "Actividad 2",
            },
          ],
        },
      ],
      marioPosition: { top: 160, left: 140 },
      marioImage: require("@/assets/mario.png"),
    };
  },
  methods: {
    playBackgroundMusic() {
      const audio = document.getElementById("backgroundMusic");
      audio.play();
    },
    isNearPosition(targetX, targetY) {
      const threshold = 80;

      return (
        Math.abs(this.marioPosition.left - targetX) < threshold &&
        Math.abs(this.marioPosition.top - targetY) < threshold
      );
    },
    moveLeft() {
      this.marioPosition.left -= 10 * 2;
      this.updateCoordinates();
    },
    moveRight() {
      this.marioPosition.left += 10 * 2;
      this.updateCoordinates();
      this.playBackgroundMusic();
    },
    moveUp() {
      this.marioPosition.top -= 10 * 2;
      this.updateCoordinates();
    },
    moveDown() {
      this.marioPosition.top += 10 * 2;
      this.updateCoordinates();
    },
    selectLevel() {
      if (
        this.isNearPosition((360 * this.initialWindowSize.width) / 2000, 20)
      ) {
        const currentLevel = this.level.find(
          (level) => level.title === "Level 1"
        );
        this.selectedLevel = currentLevel;
        console.log(this.selectedLevel);
        this.playBackgroundMusic();
      }
      if (
        this.isNearPosition((1000 * this.initialWindowSize.width) / 2000, 20)
      ) {
        const currentLevel = this.level.find(
          (level) => level.title === "Level 2"
        );
        this.selectedLevel = currentLevel;
        console.log(this.selectedLevel);
      }
      if (
        this.isNearPosition((1200 * this.initialWindowSize.width) / 2000, 20)
      ) {
        const currentLevel = this.level.find(
          (level) => level.title === "Level 3"
        );
        this.selectedLevel = currentLevel;
        console.log(this.selectedLevel);
      }
      if (
        this.isNearPosition(
          (1200 * this.initialWindowSize.width) / 2000,
          (20 * this.initialWindowSize.height) / 100
        )
      ) {
        const currentLevel = this.level.find(
          (level) => level.title === "Level 4"
        );
        this.selectedLevel = currentLevel;
        console.log(this.selectedLevel);
      }
      if (
        this.isNearPosition(
          (360 * this.initialWindowSize.width) / 2000,
          this.initialWindowSize.height - 100
        )
      ) {
        const currentLevel = this.level.find(
          (level) => level.title === "Level 5"
        );
        this.selectedLevel = currentLevel;
        console.log(this.selectedLevel);
      }
      if (
        this.isNearPosition(
          (1000 * this.initialWindowSize.width) / 2000,
          this.initialWindowSize.height - 100
        )
      ) {
        const currentLevel = this.level.find(
          (level) => level.title === "Level 6"
        );
        this.selectedLevel = currentLevel;
        console.log(this.selectedLevel);
      }
      if (
        this.isNearPosition(
          (1600 * this.initialWindowSize.width) / 2000,
          this.initialWindowSize.height - 300
        )
      ) {
        const currentLevel = this.level.find(
          (level) => level.title === "Level 7"
        );
        this.selectedLevel = currentLevel;
        console.log(this.selectedLevel);
      }
    },
    backToSelection() {
      this.selectedLevel = null;
    },
    onKeyDown(event) {
      switch (event.key) {
        case "ArrowLeft":
          this.moveLeft();
          break;
        case "ArrowRight":
          this.moveRight();
          break;
        case "ArrowUp":
          this.moveUp();
          break;
        case "ArrowDown":
          this.moveDown();
          break;
        case "Enter":
          this.selectLevel();
          break;
        default:
          break;
      }
    },
    moveToCoordinates(x, y) {
      this.marioPosition.left = x;
      this.marioPosition.top = y;
      this.updateCoordinates();
    },
    updateCoordinates() {
      const coordinatesElement = document.getElementById("coordinates");
      const windowWidth = window.innerWidth;
      const windowHeight = window.innerHeight;

      console.log("Window width:", windowWidth, "Window height:", windowHeight);

      // Calculate proportions based on the window size
      const proportionX = windowWidth / 800; // Adjust 800 according to your design
      const proportionY = windowHeight / 600; // Adjust 600 according to your design
      //const proportion = windowWidth / 800; // Adjust 800 according to your design

      // Update the size based on proportion
      //this.marioSize = Math.round(100 * proportion);
      this.marioSize = Math.round(100 * Math.min(proportionX, proportionY));
      if (
        this.initialWindowSize.width !== windowWidth ||
        this.initialWindowSize.height !== windowHeight
      ) {
        this.initialWindowSize.width = windowWidth;
        this.initialWindowSize.height = windowHeight;
        this.marioPosition.left = this.marioSize;
        this.marioPosition.top = this.marioSize;
      }

      // Update the coordinates based on proportions
      const adjustedLeft = Math.round(this.marioPosition.left * proportionX);
      const adjustedTop = Math.round(this.marioPosition.top * proportionY);

      coordinatesElement.textContent = `Coordinates: ${adjustedLeft}, ${adjustedTop}`;
    },
  },
  mounted() {
    window.addEventListener("keydown", this.onKeyDown);
    window.addEventListener("resize", this.updateCoordinates);
  },
  beforeUnmount() {
    window.removeEventListener("keydown", this.onKeyDown);
    window.removeEventListener("resize", this.updateCoordinates);
  },
};
</script>

<style scoped>
body {
  overflow: hidden;
  margin: 0;
  font-family: Arial, sans-serif;
}

#app {
  display: flex;
  flex-direction: column;
  align-items: center;
  overflow: hidden;
  overflow-x: hidden;
  overflow-y: hidden;
}

#map-container {
  position: relative;
  width: 100vw;
  height: 100vh;
  overflow: hidden;
}

#map {
  width: 100%;
  height: 100%;
}

#mario {
  position: absolute;
  width: 100px;
  height: 100px;
  cursor: pointer;
  transition: top 0.3s ease, left 0.3s ease, width 0.3s ease, height 0.3s ease;
  background: transparent;
  border: none;
  padding: 0;
  margin: 0;
}

#coordinates {
  position: absolute;
  top: 10px;
  left: 10px;
  color: white;
  font-size: 16px;
}

#level-info {
  width: 100vw;
  height: 100vh;
  position: fixed;
  top: 0;
  left: 0;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  background-size: cover;
  background-position: center;
}

#level-selection {
  margin-top: 20px;
  text-align: center;
}

#level-selection button {
  margin: 5px;
  padding: 10px;
  cursor: pointer;
}

table {
  width: 50%;
  border-collapse: collapse;
  margin-top: 20px;
}

th,
td {
  border: 1px solid #ddd;
  padding: 10px;
  text-align: left;
}

th {
  background-color: #f2f2f2;
}

.level-title {
  font-size: 20px;
  font-weight: bold;
}

.activity-row {
  background-color: #3bb143;
  color: white;
}

.activity-link {
  color: #0066cc;
  text-decoration: underline;
}
</style>
