<template>
    <div class="сlear-all-btn" @click="clearAllTask">
        <img class="сlear-all-icon" src="../assets/svg/cross-icon.svg" alt="Cross icon image" />
        <p class="clear-all-title">Очистить все</p>
    </div>
</template>

<script>
export default {
    name: "ClearAllButton",
    methods: {
        clearAllTask() {
            this.$emit("clear-allTask");
        },
    },
};
</script>

<style lang="scss">
.сlear-all-btn {
    padding: 6px 10px;
    border-radius: 10px;
    background: #ff5b5b;
    overflow: hidden;
    transition: 0.2s;
    cursor: pointer;

    display: flex;
    flex-direction: row;
    justify-content: center;
    align-items: center;
    gap: 5px;

    .clear-all-title {
        opacity: 1;
        transition: 0.1s;

        font-size: 18px;
        font-weight: 400;
        color: #fff;
    }

    .сlear-all-icon {
        transition: 0.3s;
        transform: translate3d(0, 0, 0);
    }

    &:hover {
        background: #ff3c3c;

        .clear-all-title {
            opacity: 0;
        }

        .сlear-all-icon {
            transform: rotate(180deg) translate3d(-60px, 0, 0);
        }
    }
}
</style>
