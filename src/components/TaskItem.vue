<template>
  <!--
    Conditional styling:
    - .task-item--completed  -> zauzet zadatak (drugačiji stil)
    - .task-item--high       -> zadatak visokog prioriteta (druga boja)
  -->
 <li
    class="task-item"
    :class="{
      'task-item--completed': task.completed,
      'task-item--high': task.priority === 'High',
    }"
  >
    <!-- Informacija o statusu (završeno / nije) -->
    <span
      class="task-item__status"
      aria-hidden="true"
      :title="task.completed ? 'Završeno' : 'Nije završeno'"
    >
      {{ task.completed ? "✅" : "⭕" }}
    </span>

    <div class="task-item__body">
      <span class="task-item__title">{{ task.title }}</span>

      <div class="task-item__meta">
        <span class="badge" :class="priorityClass">{{ priorityLabel }}</span>
        <span class="task-item__state">
          {{ task.completed ? "Završeno" : "Nije završeno" }}
        </span>
      </div>
    </div>

    <div class="task-item__actions">
      <button
        class="task-item__btn task-item__btn--toggle"
        :disabled="task.completed"
        :aria-label="task.completed ? 'Zadatak je završen' : 'Označi zadatak kao završen'"
        @click="$emit('toggle-complete', task.id)"
      >
        {{ task.completed ? "Završeno" : "Završi" }}
      </button>

      <button
        class="task-item__btn task-item__btn--danger"
        :aria-label="`Izbriši zadatak: ${task.title}`"
        @click="$emit('delete-task', task.id)"
      >
        Izbriši
      </button>
    </div>
 </li>
</template>

<script>
export default {
  name: "TaskItem",
  // Podaci se prosleđuju iz roditeljske (App) komponente preko propsa
  props: {
    task: {
      type: Object,
      required: true,
    },
    // Prevod naziva prioriteta dolazi iz App.vue kako se logika
    // prevođenja ne bi duplirala u više komponenti.
    priorityLabel: {
      type: String,
      default: "",
    },
  },
  // Eksplicitno deklarisani događaji koje komponenta emituje
  emits: ["toggle-complete", "delete-task"],
  computed: {
    priorityClass() {
      const classes = {
        High: "badge--high",
        Medium: "badge--medium",
        Low: "badge--low",
      };
      return classes[this.task.priority] || "";
    },
  },
};
</script>

<style scoped>
.task-item {
  display: flex;
  align-items: center;
  gap: 14px;
  background: #ffffff;
  border-left: 5px solid #cbd5e1;
  border-radius: 10px;
  padding: 14px 18px;
  margin-bottom: 10px;
  box-shadow: 0 2px 8px rgba(15, 23, 42, 0.06);
  transition: transform 0.1s, box-shadow 0.15s;
}

.task-item:hover {
  transform: translateY(-1px);
  box-shadow: 0 6px 16px rgba(15, 23, 42, 0.1);
}

/* Zadatak visokog prioriteta istaknut crvenom bojom */
.task-item--high {
  border-left-color: #ef4444;
  background: #fef2f2;
}

/* Završen zadatak - drugačiji (prigušen) stil, ali i dalje čitljiv */
.task-item--completed {
  border-left-color: #22c55e;
  background: #f0fdf4;
}

.task-item--completed .task-item__status,
.task-item--completed .task-item__meta {
  opacity: 0.65;
}

.task-item--completed .task-item__title {
  text-decoration: line-through;
  color: #64748b;
}

.task-item__status {
  font-size: 20px;
  line-height: 1;
}

.task-item__body {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 6px;
  min-width: 0;
}

.task-item__title {
  font-size: 16px;
  font-weight: 600;
  color: #1e293b;
  overflow-wrap: anywhere;
}

.task-item__meta {
  display: flex;
  align-items: center;
  gap: 10px;
  flex-wrap: wrap;
}

.task-item__state {
  font-size: 12px;
  color: #64748b;
}

/* Bedž prioriteta */
.badge {
  font-size: 11px;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 0.4px;
  padding: 3px 9px;
  border-radius: 999px;
}

.badge--high {
  background: #ef4444;
  color: #fff;
}

.badge--medium {
  background: #f59e0b;
  color: #fff;
}

.badge--low {
  background: #10b981;
  color: #fff;
}

/* Dugmad */
.task-item__actions {
  display: flex;
  gap: 8px;
  flex-shrink: 0;
}

.task-item__btn {
  padding: 8px 14px;
  border: none;
  border-radius: 8px;
  font-size: 13px;
  font-weight: 600;
  cursor: pointer;
  transition: background 0.15s, opacity 0.15s;
  white-space: nowrap;
}

.task-item__btn--toggle {
  background: #6366f1;
  color: #fff;
}

.task-item__btn--toggle:hover:not(:disabled) {
  background: #4f46e5;
}

.task-item__btn--toggle:disabled {
  background: #22c55e;
  cursor: default;
  opacity: 0.9;
}

.task-item__btn--danger {
  background: #fee2e2;
  color: #b91c1c;
}

.task-item__btn--danger:hover {
  background: #fecaca;
}
</style>
