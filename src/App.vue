<template>
    <div class="logo-container">
        <img class="logo-image" src="./assets/svg/main-logo.svg" alt="Logo image" />
    </div>
    <div class="input-form-container">
        <TaskInputForm @taskAdded="addNewTask" />
    </div>
    <div class="task-container">
        <div v-if="tasks.length === 0">
            <p class="title-empty-container">Нет активных задач</p>
        </div>
        <div class="pinned-task" v-if="hasPinnedTasks">
            <p class="task-status">Закрепленные</p>
            <TaskCard
                v-for="task in pinnedTasks"
                :key="task.id"
                :task="task"
                @completed-task="completedBtn(task.id)"
                @pin-task="pinBtn(task.id)"
                @delete-task="deleteBtn(task.id)"
            />
        </div>
        <div class="active-task" v-if="hasActiveTasks">
            <p class="task-status">Активные</p>
            <TaskCard
                v-for="task in activeTasks"
                :key="task.id"
                :task="task"
                @completed-task="completedBtn(task.id)"
                @pin-task="pinBtn(task.id)"
                @delete-task="deleteBtn(task.id)"
            />
        </div>
        <div class="completed-task" v-if="hasCompletedTasks">
            <p class="task-status">Выполненные</p>
            <TaskCard
                v-for="task in completedTasks"
                :key="task.id"
                :task="task"
                @completed-task="completedBtn(task.id)"
                @pin-task="pinBtn(task.id)"
                @delete-task="deleteBtn(task.id)"
            />
        </div>
        <ClearAllButton @clear-allTask="clearAllTask" v-if="tasks.length > 0" />
    </div>
    <GitHubRepoButton />
</template>

<script>
import TaskCard from "./components/TaskCard";
import TaskInputForm from "./components/TaskInputForm.vue";
import ClearAllButton from "./components/ClearAllButton.vue";
import GitHubRepoButton from "./components/GitHubRepoButton.vue";

export default {
    name: "App",
    components: {
        TaskCard,
        TaskInputForm,
        ClearAllButton,
        GitHubRepoButton,
    },
    data() {
        return {
            tasks: [],
            taskIdCounter: 0,
        };
    },
    computed: {
        activeTasks() {
            return this.tasks.filter((task) => task.status === "active");
        },
        pinnedTasks() {
            return this.tasks.filter((task) => task.status === "pinned");
        },
        completedTasks() {
            return this.tasks.filter((task) => task.status === "completed");
        },
        hasActiveTasks() {
            return this.activeTasks.length > 0;
        },
        hasPinnedTasks() {
            return this.pinnedTasks.length > 0;
        },
        hasCompletedTasks() {
            return this.completedTasks.length > 0;
        },
    },
    methods: {
        addNewTask(newTask) {
            newTask.id = this.taskIdCounter++;
            newTask.status = "active";
            this.tasks.unshift(newTask);
            this.saveTasks();
        },
        completedBtn(taskId) {
            const taskIndex = this.tasks.findIndex((task) => task.id === taskId);
            if (taskIndex !== -1) {
                this.tasks[taskIndex].status = "completed";
                this.saveTasks();
            }
        },
        pinBtn(taskId) {
            const taskIndex = this.tasks.findIndex((task) => task.id === taskId);
            if (taskIndex !== -1) {
                this.tasks[taskIndex].status = "pinned";
                this.saveTasks();
            }
        },
        deleteBtn(taskId) {
            const index = this.tasks.findIndex((task) => task.id === taskId);
            if (index !== -1) {
                this.tasks.splice(index, 1);
                this.saveTasks();
            }
        },
        clearAllTask() {
            if (window.matchMedia("(pointer: coarse)").matches) {
                setTimeout(() => {
                    this.tasks = [];
                    localStorage.removeItem("tasks");
                }, 290);
            } else {
                this.tasks = [];
                localStorage.removeItem("tasks");
            }
        },
        saveTasks() {
            localStorage.setItem("tasks", JSON.stringify(this.tasks));
        },
    },
    mounted() {
        const savedTasks = localStorage.getItem("tasks");
        if (savedTasks) {
            this.tasks = JSON.parse(savedTasks);
            this.taskIdCounter = Math.max(...this.tasks.map((task) => task.id)) + 1;
        }
    },
};
</script>

<style lang="scss">
@import "./styles/media-queries.scss";

* {
    margin: 0;
    box-sizing: border-box;
}

html {
    scroll-behavior: smooth;
}

a {
    text-decoration: none;
}

::-webkit-scrollbar {
    width: 8px;
}

::-webkit-scrollbar-thumb {
    border-radius: 5px;
    background: #2166ec;
}

body {
    min-width: 320px;
    background: #fff;
    padding: 20px;
    overflow-y: scroll;
    overflow-x: hidden;

    font-family: "Roboto", sans-serif;
    font-size: 16px;
    font-weight: 400;
    color: #000;

    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
}

.logo-container {
    margin-bottom: 25px;
    display: flex;
    justify-content: center;
}

.input-form-container {
    position: sticky;
    width: 900px;
    top: 0;
    left: 0;

    z-index: 999;
    padding: 25px 0px 50px 0px;
    background: #fff;
}

.task-container {
    width: 900px;
    min-height: calc(100dvh - 335px);
    margin-bottom: 50px;

    display: flex;
    flex-direction: column;
    justify-content: flex-start;
    align-items: center;
    gap: 30px;
}

.title-empty-container {
    font-size: 22px;
    font-weight: 400;
    color: #888;
    text-align: center;
}

.pinned-task,
.active-task,
.completed-task {
    width: 100%;
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
    align-items: flex-start;
    gap: 15px;
}

.task-status {
    margin-bottom: 5px;
    font-size: 20px;
    font-weight: 500;
    color: #000;
}
</style>
