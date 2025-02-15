<!-- eslint-disable vue/multi-word-component-names -->
<!-- eslint-disable vue/multi-word-component-names -->
<template>
  <div class="dashboard-container">
    <!-- 📦 Section Widgets -->
    <div class="widgets-container">
      <div class="widget fade-in">
        <h3>🚛 Collectes Aujourd’hui</h3>
        <p class="big-number">{{ totalCollections }}</p>
        <p>+{{ additionalCollections }} collectes supplémentaires</p>
      </div>

      <div class="widget fade-in">
        <h3>♻️ Taux de Recyclage</h3>
        <PieChart :data="recyclingRateData" />
      </div>

      <div class="widget fade-in">
        <h3>🌍 Pollution Évitée</h3>
        <BarChart :data="pollutionData" />
      </div>
    </div>

    <!-- 📈 Graphiques & Tâches -->
    <div class="graph-tasks-container">
      <div class="graph fade-in">
        <h3>📈 Évolution du Recyclage</h3>
        <LineChart :data="recyclingTrend" />
      </div>

      <div class="tasks fade-in">
        <h3>📝 Tâches du Jour</h3>
        <ul>
          <li v-for="task in tasks" :key="task.id">✅ {{ task.name }} - {{ task.time }}</li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from "vue";
import { useRouter } from "vue-router";
import PieChart from "@/components/PieChart.vue";
import BarChart from "@/components/BarChart.vue";
import LineChart from "@/components/LineChart.vue";

// ✅ Définition des types
interface Task {
  id: number;
  name: string;
  time: string;
}

// ✅ Définition des variables réactives avec types explicites
const totalCollections = ref<number>(0);
const additionalCollections = ref<number>(0);
const recyclingRateData = ref<number[]>([]);
const pollutionData = ref<number[]>([]);
const recyclingTrend = ref<number[]>([]);
const tasks = ref<Task[]>([]);


    const router = useRouter();

    onMounted(() => {
      const token = localStorage.getItem('token');
      if (!token) {
        router.push('/login');  // Redirige vers la connexion si aucun token
      }
    });

// ✅ Simulation de récupération de données API
const fetchData = async () => {
  try {
    // Simuler un appel API avec setTimeout
    setTimeout(() => {
      totalCollections.value = 18;
      additionalCollections.value = 5;
      recyclingRateData.value = [40, 30, 20, 10]; // Ex: verre, plastique, métal, autres
      pollutionData.value = [100, 150, 200, 250]; // Ex: données de pollution
      recyclingTrend.value = [10, 20, 30, 50, 80, 100]; // Ex: tendance recyclage
      tasks.value = [
        { id: 1, name: "Ramassage Fidjrossè", time: "08h00" },
        { id: 2, name: "Maintenance Camions", time: "14h00" },
        { id: 3, name: "Tri des Déchets", time: "16h30" },
      ];
    }, 1000);
  } catch (error) {
    console.error("Erreur de chargement des données :", error);
  }
};

// ✅ Charger les données au montage
onMounted(fetchData);
</script>

<style scoped>
/* 🌿 Conteneur principal */
.dashboard-container {
  padding: 20px;
  background: linear-gradient(120deg, #e0f7fa, #f1f8e9);
  min-height: 100vh;
}

/* 📦 Widgets */
.widgets-container {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 20px;
  margin-top: 20px;
}

.widget {
  background: white;
  padding: 20px;
  border-radius: 15px;
  box-shadow: 0px 5px 15px rgba(0, 0, 0, 0.1);
  text-align: center;
  transition: transform 0.3s ease-in-out;
}

.widget:hover {
  transform: scale(1.05);
}

.big-number {
  font-size: 2.5rem;
  font-weight: bold;
  color: #01712b;
  margin-top: 10px;
}

/* 📈 Graphiques et Tâches */
.graph-tasks-container {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
  margin-top: 20px;
}

.graph, .tasks {
  flex: 1;
  background: white;
  padding: 20px;
  border-radius: 15px;
  box-shadow: 0px 5px 15px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s ease-in-out;
}

.graph:hover, .tasks:hover {
  transform: scale(1.03);
}

/* 🎨 Animations */
@keyframes fadeIn {
  from { opacity: 0; transform: translateY(10px); }
  to { opacity: 1; transform: translateY(0); }
}

.fade-in {
  animation: fadeIn 1s ease-in-out;
}
</style>


