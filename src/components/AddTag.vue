<script setup>
import { ref } from 'vue';
import { useRouter } from "vue-router";
import { nanoid } from 'nanoid';
import { Notify } from 'quasar';
import { useTransactionsStore } from '../stores/transactions';

const transactionsStore = useTransactionsStore();

const router = useRouter();

const tagName = ref("");

function handleSubmit() {
    try {
        transactionsStore.addTag({
            id: nanoid(),
            name: tagName.value,
        })

        router.push('/tags');

        Notify.create({
            message: 'Transaction Added Successfully',
            type: "positive",
            actions: [
                { icon: 'close', color: 'white', round: true, }
            ]
        })

    } catch (error) {
        Notify.create({
            message: error.message,
            type: "negative",
            actions: [
                { icon: 'close', color: 'white', round: true, }
            ]
        })
    }
}
</script>

<template>
    <q-page class="flex flex-center column q-my-lg">
        <q-card flat bordered class="form-card">
            <q-card-section class="row items-center q-pb-none">
                <div class="text-h6 q-mb-md">Add Tag</div>
                <q-space />
            </q-card-section>

            <q-card-section>
                <q-form @submit.prevent="handleSubmit">
                    <q-input filled v-model="tagName" label="Tag Name" required lazy-rules
                        :rules="[val => val && val.length > 0 || 'Tag Name is required']" autofocus />

                    <q-page-sticky position="bottom-right" :offset="[18, 18]">
                        <q-btn type="submit" fab icon="done" color="primary">
                        </q-btn>
                    </q-page-sticky>

                    <q-page-sticky position="bottom-left" :offset="[18, 18]">
                        <q-btn :to="{ 'name': 'tags' }" fab icon="undo" color="negative">
                        </q-btn>
                    </q-page-sticky>
                </q-form>
            </q-card-section>

        </q-card>
    </q-page>
</template>
<style lang="sass">
.form-card
  width: 100%
  max-width: 400px

.q-dark div,
.body--dark div
  .multiselect__tags
    background-color: rgb(24, 26, 27)
    color: white
  .multiselect__input
    background-color: rgb(24, 26, 27)
    color: white
  .multiselect__content-wrapper
      background-color: rgb(24, 26, 27)
      color: white
  .multiselect__placeholder
    color: white
</style>