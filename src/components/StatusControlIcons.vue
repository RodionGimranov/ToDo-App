<template>
    <div class="controls-container">
        <span
            class="completed-btn"
            @click="handleCompletedClick"
            :class="{ clicked: colorCompletedIcon }"
        ></span>
        <span class="pin-btn" @click="handlePinClick" :class="{ clicked: colorPinIcon }"></span>
        <span class="delete-btn" @click="handleDeleteClick"></span>
    </div>
</template>

<script>
export default {
    name: "StatusControlIcons",
    props: {
        task: { type: Object, required: true },
        colorCompletedIcon: { type: Boolean, required: true },
        colorPinIcon: { type: Boolean, required: true },
    },
    methods: {
        handleCompletedClick() {
            this.$emit("completed-clicked");
        },
        handlePinClick() {
            this.$emit("pin-clicked");
        },
        handleDeleteClick() {
            this.$emit("delete-clicked");
        },
    },
};
</script>

<style lang="scss">
.controls-container {
    height: 44px;
    margin-right: 10px;

    display: flex;
    flex-direction: row;
    justify-content: flex-start;
    align-items: center;
    gap: 8px;
}

.completed-btn,
.pin-btn,
.delete-btn {
    width: 20px;
    height: 20px;

    border-radius: 100%;
    transition: 0.2s;
    cursor: pointer;
}

.completed-btn {
    border: 3px solid #07d110;

    &:hover {
        background: #07d110;
    }

    &.clicked {
        background: #07d110;
    }
}

.pin-btn {
    border: 3px solid #fae560;

    &:hover {
        background: #fae560;
    }

    &.clicked {
        background-color: #fae560;
    }
}

.delete-btn {
    border: 3px solid #ff5b5b;

    &:hover {
        background: #ff5b5b;
    }
}
</style>
