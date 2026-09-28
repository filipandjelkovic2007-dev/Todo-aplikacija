<template>
 <div class="app">
    <header class="app__header">
      <h1>📝 To Do Aplikacija</h1>
      <p class="app__subtitle">Organizuj svoje zadatke po prioritetu i statusu.</p>
    </header>

    <!-- Forma za dodavanje novog zadatka -->
    <section class="card add-task">
      <h2 class="section-title">Novi zadatak</h2>

      <div class="add-task__row">
        <input
          v-model="newTaskTitle"
          class="add-task__input"
          type="text"
          placeholder="Unesi naziv zadatka..."
          @keyup.enter="addTask"
        />

        <select v-model="newTaskPriority" class="add-task__select">
          <option value="High">Visoko</option>
          <option value="Medium">Srednje</option>
          <option value="Low">Nisko</option>
        </select>

        <button class="add-task__btn" @click="addTask">Dodaj zadatak</button>
      </div>
    </section>

    <!-- Filteri -->
    <section class="card filters">
      <div class="filters__group">
        <span class="filters__label">Status:</span>
        <button
          v-for="option in statusFilters"
          :key="option"
          class="chip"
          :class="{ 'chip--active': statusFilter === option }"
          @click="statusFilter = option"
        >
          {{ statusLabels[option] }}
        </button>
      </div>

      <div class="filters__group">
        <span class="filters__label">Prioritet:</span>
        <button
          v-for="option in priorityFilters"
          :key="option"
          class="chip"
          :class="{ 'chip--active': priorityFilter === option }"
          @click="priorityFilter = option"
        >
          {{ priorityLabels[option] }}
        </button>
      </div>
    </section>

    <!-- Statistika -->
    <section v-if="tasks.length > 0" class="stats">
      <span>Ukupno: <strong>{{ tasks.length }}</strong></span>
      <span>Završeno: <strong>{{ completedCount }}</strong></span>
      <span>Aktivno: <strong>{{ activeCount }}</strong></span>
    </section>

    <!-- Lista zadataka -->
    <ul class="task-list">
      <TaskItem
        v-for="task in filteredTasks"
        :key="task.id"
        :task="task"
        :priority-label="priorityLabels[task.priority]"
        @toggle-complete="toggleComplete"
        @delete-task="deleteTask"
      />
    </ul>

    <!-- Poruka kada nema zadataka -->
    <p v-if="filteredTasks.length === 0" class="empty-state">
      Nema dostupnih zadataka.
    </p>
 </div>
</template>

<script>
import TaskItem from "./components/TaskItem.vue";

export default {
  name: "App",
  components: {
    TaskItem,
  },
  data() {
    return {
      // Početni niz zadataka
      tasks: [
        { id: 1, title: "Learn Vue basics", completed: true, priority: "High" },
        { id: 2, title: "Practice Vue directives", completed: false, priority: "Medium" },
        { id: 3, title: "Create To Do App", completed: false, priority: "Low" },
      ],

      // Polja vezana za formu preko v-model
      newTaskTitle: "",
      newTaskPriority: "Medium",

      // Filteri
      statusFilter: "All", // Svi | aktivno | završeno
      priorityFilter: "All", // Svi | High | Medium | Low
      statusFilters: ["All", "active", "completed"],
      priorityFilters: ["All", "High", "Medium", "Low"],

      statusLabels: {
        All: "Svi",
        active: "Aktivno",
        completed: "Završeno",
      },

      priorityLabels: {
        All: "Svi",
        High: "Visoko",
        Medium: "Srednje",
        Low: "Nisko",
      },
    };
  },
  computed: {
    completedCount() {
      return this.tasks.filter((task) => task.completed).length;
    },
    activeCount() {
      return this.tasks.filter((task) => !task.completed).length;
    },
    filteredTasks() {
      return this.tasks.filter((task) => {
        // Filter po statusu
        const statusOk =
          this.statusFilter === "All" ||
          (this.statusFilter === "completed" && task.completed) ||
          (this.statusFilter === "active" && !task.completed);

        // Filter po prioritetu
        const priorityOk =
          this.priorityFilter === "All" || task.priority === this.priorityFilter;

        return statusOk && priorityOk;
      });
    },
  },
  methods: {
    addTask() {
      const title = this.newTaskTitle.trim();

      // Ako je input prazan, ne dodajemo zadatak
      if (!title) {
        return;
      }

      // Novi jedinstveni ID (najveći postojeći ID + 1)
      const newId =
        this.tasks.length > 0
          ? Math.max(...this.tasks.map((task) => task.id)) + 1
          : 1;

      this.tasks.push({
        id: newId,
        title,
        completed: false,
        priority: this.newTaskPriority,
      });

      // Nakon dodavanja isprazni input
      this.newTaskTitle = "";
      this.newTaskPriority = "Medium";
    },

    toggleComplete(id) {
      const task = this.tasks.find((task) => task.id === id);
      if (task) {
        task.completed = !task.completed;
      }
    },

    deleteTask(id) {
      this.tasks = this.tasks.filter((task) => task.id !== id);
    },
  },
};
</script>

<style>
/* ===== Globalni stilovi ===== */
* {
  box-sizing: border-box;
}

body {
  margin: 0;
  font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
  background: linear-gradient(135deg, #eef2ff 0%, #f8fafc 100%);
  color: #1e293b;
}

.app {
  max-width: 760px;
  margin: 0 auto;
  padding: 32px 20px 60px;
}

/* ===== Header ===== */
.app__header {
  text-align: center;
  margin-bottom: 24px;
}

.app__header h1 {
  margin: 0 0 6px;
  font-size: 32px;
  color: #312e81;
}

.app__subtitle {
  margin: 0;
  color: #64748b;
  font-size: 14px;
}

/* ===== Kartice ===== */
.card {
  background: #ffffff;
  border-radius: 12px;
  padding: 18px 20px;
  margin-bottom: 18px;
  box-shadow: 0 4px 14px rgba(15, 23, 42, 0.07);
}

.section-title {
  margin: 0 0 12px;
  font-size: 16px;
  color: #334155;
}

/* ===== Forma ===== */
.add-task__row {
  display: flex;
  gap: 10px;
  flex-wrap: wrap;
}

.add-task__input {
  flex: 1 1 220px;
  padding: 10px 14px;
  border: 1px solid #cbd5e1;
  border-radius: 8px;
  font-size: 14px;
  outline: none;
  transition: border-color 0.15s, box-shadow 0.15s;
}

.add-task__input:focus {
  border-color: #6366f1;
  box-shadow: 0 0 0 3px rgba(99, 102, 241, 0.15);
}

.add-task__select {
  padding: 10px 12px;
  border: 1px solid #cbd5e1;
  border-radius: 8px;
  font-size: 14px;
  background: #fff;
  cursor: pointer;
}

/* ===== Dugmad ===== */
.add-task__btn {
  padding: 10px 18px;
  border: none;
  border-radius: 8px;
  font-size: 14px;
  font-weight: 600;
  cursor: pointer;
  transition: background 0.15s, transform 0.05s;
  background: #6366f1;
  color: #fff;
}

.add-task__btn:active {
  transform: scale(0.98);
}

.add-task__btn:hover {
  background: #4f46e5;
}

/* ===== Filteri ===== */
.filters {
  display: flex;
  flex-wrap: wrap;
  gap: 18px;
}

.filters__group {
  display: flex;
  align-items: center;
  gap: 8px;
  flex-wrap: wrap;
}

.filters__label {
  font-size: 13px;
  font-weight: 600;
  color: #64748b;
}

.chip {
  padding: 6px 14px;
  border: 1px solid #cbd5e1;
  border-radius: 999px;
  background: #f8fafc;
  font-size: 13px;
  color: #475569;
  cursor: pointer;
  transition: all 0.15s;
}

.chip:hover {
  background: #eef2ff;
}

.chip--active {
  background: #6366f1;
  border-color: #6366f1;
  color: #fff;
}

/* ===== Statistika ===== */
.stats {
  display: flex;
  gap: 18px;
  justify-content: center;
  margin-bottom: 16px;
  font-size: 14px;
  color: #475569;
}

.stats strong {
  color: #312e81;
}

/* ===== Lista ===== */
.task-list {
  list-style: none;
  margin: 0;
  padding: 0;
}

/* ===== Prazno stanje ===== */
.empty-state {
  text-align: center;
  padding: 28px;
  background: #ffffff;
  border: 2px dashed #cbd5e1;
  border-radius: 12px;
  color: #64748b;
  font-size: 15px;
}
</style>
