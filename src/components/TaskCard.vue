<template>
    <div class="task-card">
        <span class="status-color" :style="{ backgroundColor: statusColor }"></span>
        <div class="task-text-container" :style="{ color: textColor }">{{ task.text }}</div>
        <StatusControlIcons
            :task="task"
            :colorCompletedIcon="task.status === 'completed'"
            :colorPinIcon="task.status === 'pinned'"
            @completed-clicked="handleCompletedClicked"
            @pin-clicked="handlePinClicked"
            @delete-clicked="handleDeleteClicked"
        />
    </div>
</template>

<script>
import StatusControlIcons from "./StatusControlIcons.vue";

export default {
    name: "TaskCard",
    components: {
        StatusControlIcons,
    },
    props: {
        task: { type: Object, required: true },
    },
    computed: {
        statusColor() {
            if (this.task.status === "completed") {
                return "#07D110";
            } else if (this.task.status === "pinned") {
                return "#FFEB49";
            } else {
                return "#2166ec";
            }
        },
        textColor() {
            if (this.task.status === "completed") {
                return "#999";
            } else {
                return "#000";
            }
        },
    },
    methods: {
        handleCompletedClicked() {
            this.$emit("completed-task", this.task.id);
        },
        handlePinClicked() {
            this.$emit("pin-task", this.task.id);
        },
        handleDeleteClicked() {
            this.$emit("delete-task", this.task.id);
        },
    },
};
</script>

<style lang="scss">
.task-card {
    position: relative;
    width: 100%;
    min-height: 44px;

    overflow: hidden;
    background: #eff1f2;
    border-radius: 10px;
    animation: cardAnimation 1s ease-out;

    display: flex;
    flex-direction: row;
    justify-content: flex-start;
    align-items: flex-start;

    &:hover {
        box-shadow: 0px 2px 8px rgba(0, 0, 0, 0.25);
    }
}

@keyframes cardAnimation {
    from {
        opacity: 0.2;
    }
    to {
        opacity: 1;
    }
}

.status-color {
    position: absolute;
    width: 8px;
    height: 100%;
}

.task-text-container {
    width: 100%;
    margin: 10px 10px 10px 18px;

    display: flex;
    flex-wrap: wrap;

    font-size: 20px;
    font-weight: 400;
}
</style>
