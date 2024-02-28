<template>
    <div>
        <div class="main">
            <div class="main__events">
                <div class="main__events--title">
                    <h2>Events {{ store.events.length ? store.events.length : '' }}</h2>
                    <div @click="showModal">
                        +
                    </div>
                </div>
                <template v-if="store.events.length > 0">
                    <div v-for="(event, index) in store.events" :key="event.id" class="main__events--content row"
                        :class="{ 'checked': event.done }">
                        <EventListItem :index="index" :eventItem="event" @deleteEvent="deleteEvent" @editEvent="editEvent"
                            @checkedEvent="checkedEvent"></EventListItem>
                    </div>
                </template>
                <template v-else>
                    <div>No events</div>
                </template>
            </div>
        </div>
    </div>
    <EventPopup :visible="isVisible" @closeModal="closeModal">
        <template #form>
            <h2>Add event</h2>
            <div>
                <input type="text" placeholder="Title" v-model="formData.title">
            </div>
            <div>
                <textarea placeholder="Description" v-model="formData.description"></textarea>
            </div>
            <div class="action">
                <button type="submit" @click="addEvent">Add</button>
            </div>
            <button @click="closeModal" class="close">
                <img :src="closeSvg" alt="Close">
            </button>
        </template>
    </EventPopup>
    <EventPopup :visible="editModalVisible" @closeModal="closeModal">
        <template #form>
            <h2>Edit event</h2>
            <div>
                <label for="title">Title</label>
                <input type="text" id="title" placeholder="Title" v-model="editFormData.title">
            </div>
            <div>
                <label for="description">Descripiton</label>
                <textarea placeholder="Description" id="description" v-model="editFormData.description"></textarea>
            </div>
            <div class="action">
                <button type="submit" @click="confirmEvent">Confirm</button>
            </div>
            <button @click="closeModal" class="close">
                <img :src="closeSvg" alt="Close">
            </button>
        </template>
    </EventPopup>
</template>
<script setup>
import { onMounted, ref } from 'vue'
import { useEventsStore } from '@/stores/events'
import EventListItem from './EventListItem.vue';
import EventPopup from '@/components/popups/EventPopup.vue'
import closeSvg from "../../assets/close.svg"
const store = useEventsStore()

const isVisible = ref(false)
const editModalVisible = ref(false)
const initialFormData = {
    title: "",
    description: "",
}
const formData = ref(initialFormData)

const editFormData = ref(initialFormData)
const addEvent = () => {
    store.addEvent({ ...formData.value, done: false })
    closeModal()
}
const deleteEvent = (id) => {
    store.deleteEvent(id)
}
const editEvent = (id) => {
    editModalVisible.value = true
    editFormData.value = store.events.find((event) => event.id === id)
}
const checkedEvent = (id) => {
    store.checkedEvent(id)
}

const showModal = () => {
    isVisible.value = true
}

const confirmEvent = () => {
    store.editEvent(editFormData.value)
    closeModal()
}

const closeModal = () => {
    isVisible.value = false
    editModalVisible.value = false
    formData.value = initialFormData
    editFormData.value = initialFormData
}
onMounted(() => {
    store.loadEvents()
})


</script>
<style lang="scss" scoped>
// form {
//     display: flex;
//     flex-direction: column;
//     gap: 15px;


// }

.main {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    width: calc(100% - 50px);
    padding: 0 25px;
    gap: 15px;

    &__events {
        width: 70%;
        margin: 0 auto;
        padding: 25px 40px;

        &--title {
            display: flex;
            flex-direction: row;
            justify-content: space-between;
            padding: 15px 25px;

            div {
                font-size: 22px;
                cursor: pointer;
                padding: 5px;
                width: 55px;
                border-radius: 25px;
                transition: 0.3s ease-in;
                color: black !important;
                background: #efefef;

                &:hover {
                    color: white !important;
                    background: gray;
                }
            }
        }

    }
}

.checked {
    text-decoration: line-through;
}

.close {
    position: absolute;
    right: 10px;
    top: 15px;
    border: none;
    background: unset;
    cursor: pointer;
    transition: 0.5s ease-in;

    &:hover {
        transform: rotate(270deg);
    }

    img {
        width: 20px;
    }
}

.switchBlock {
    position: fixed;
    left: 55px;
    bottom: 85px;
}
</style>