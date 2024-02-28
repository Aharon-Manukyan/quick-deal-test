<template>
    <div class="modal" v-if="visible">
        <div class="modal__content">

            <slot name="form"></slot>

        </div>
    </div>
</template>

<script setup>

const props = defineProps({
    visible: {
        type: Boolean,
        required: true,
        default: false
    }
})

</script>

<style  lang="scss">
.modal {
    z-index: 1000;
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.5);
    display: flex;
    justify-content: center;
    align-items: center;

}

.modal__content {
    width: 500px;
    height: 400px;
    background-color: #fff;
    padding: 20px;
    border-radius: 8px;
    display: flex;
    flex-direction: column;
    gap: 15px;
    position: relative;

    div {
        input {
            width: 100%;
            height: 30px;
        }

        textarea {
            resize: none;
            width: 100%;
            height: 155px;
        }
    }

    .action {
        display: flex;
        flex-direction: row;
        justify-content: flex-end;

        button {
            background: rgb(158, 239, 38);
            padding: 15px 25px;
            cursor: pointer;


            &:hover {
                background: rgb(37, 242, 119);
                color: rgba(0, 0, 0, 0.5);
                border: unset;
                font-size: 16px;
            }

            transition: 0.3s ease-in;
        }
    }
}
</style>