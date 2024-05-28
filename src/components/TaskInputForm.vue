<template>
    <div
        class="control-container"
        :class="{ __focused: isFocused, __complete: isComplete, '__with-error': error }"
    >
        <input
            type="text"
            id="input"
            ref="input"
            v-model="taskText"
            @input="inputValue"
            @focus="focus"
            @blur="blur"
            @keyup.enter="addNewTask"
        />
        <label for="input" class="placeholder">{{ placeholder }}</label>
        <span class="error-message" v-show="error">Поле должно быть заполненным</span>
        <button class="create-task" @click="addNewTask">Создать</button>
    </div>
</template>

<script>
export default {
    name: "TaskInputForm",
    data() {
        return {
            taskText: "",
            error: false,
            isFocused: false,
            placeholder: "Введите текст задачи",
        };
    },
    methods: {
        focus() {
            this.isFocused = true;
        },
        blur() {
            this.isFocused = false;
        },
        inputValue() {
            this.error = false;
        },
        addNewTask() {
            if (this.taskText.trim() === "") {
                this.error = true;
            } else {
                this.error = false;
                this.$emit("taskAdded", { text: this.taskText });
                this.taskText = "";
            }
        },
    },
    computed: {
        isComplete() {
            return this.taskText.trim() !== "" && !this.isFocused;
        },
    },
};
</script>

<style lang="scss">
.control-container {
    position: relative;
    width: 100%;
    height: 47px;

    background: #fff;
    border: 1px solid #747775;
    transition: 0.2s;
    border-radius: 10px;

    display: flex;
    flex-direction: row;
    justify-content: flex-start;
    align-items: center;
    gap: 10px;

    input {
        all: unset;
        width: 100%;
        height: 100%;

        padding-left: 20px;

        font-size: 18px;
        font-weight: 400;
        color: #000;
    }

    .placeholder {
        position: absolute;
        top: 0;
        left: 20px;

        transition: 0.2s;
        transform: translate3d(0, 13px, 0);
        pointer-events: none;
        background: #fff;

        font-size: 18px;
        font-weight: 400;
        color: #888;
    }

    .error-message {
        position: absolute;
        top: 57px;
        left: 0;

        font-size: 16px;
        font-weight: 400;
        color: #ff5b5b;
    }

    .create-task {
        all: unset;
        height: 30px;

        padding: 8px 10px;
        border-radius: 0px 9px 9px 0px;
        background: #2166ec;
        cursor: pointer;
        transition: 0.2s;

        font-size: 18px;
        font-weight: 400;
        color: #fff;

        &:hover {
            background: #206bff;
        }

        &:active {
            background: #0055ff;
        }
    }
}

.__focused.control-container {
    border: 1px solid #2166ec;

    .placeholder {
        font-size: 16px;
        color: #2166ec;

        padding: 0px 5px;
        transform: translate3d(-5px, -11px, 0);
    }
}

.__complete.control-container {
    border: 1px solid #747775;

    .placeholder {
        font-size: 16px;
        color: #888;

        padding: 0px 5px;
        transform: translate3d(-5px, -13px, 0);
    }
}

.__with-error.control-container {
    border: 1px solid #ff5b5b;

    .placeholder {
        color: #ff5b5b;
    }

    .create-task {
        background: #ff5b5b;
        cursor: default;
    }
}
</style>
