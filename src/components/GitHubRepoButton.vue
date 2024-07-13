<template>
    <footer>
        <a href="https://github.com/RodionGimranov/ToDo-App.git" class="project-link" target="_blank"
            >Проект на GitHub</a
        >
    </footer>
</template>

<script>
export default {
    name: "GitHubRepoButton",
};
</script>

<style lang="scss">
footer {
    padding: 20px 0px 0px 0px;
    display: flex;
    justify-content: center;
    align-items: center;
}

.project-link {
    font-size: 18px;
    font-weight: 500;
    color: #999;
    transition: 0.2s;

    &:hover {
        color: #888;
    }
}
</style>
