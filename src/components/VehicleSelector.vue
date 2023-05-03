<template>
    <div class="flex flex-wrap justify-center">
        <div class="mx-2">
        <select class="block w-full py-2 px-3 border border-gray-300 bg-white rounded-md shadow-sm focus:outline-none focus:ring-indigo-500 focus:border-indigo-500" v-model="selectedYear" @change="getMakes">
            <option value="">Select a year</option>
            <option v-for="year in years" :key="year.id" :value="year">{{ year.year }}</option>
        </select>
        </div>

        <div class="mx-2">
        <select class="block w-full py-2 px-3 border border-gray-300 bg-white rounded-md shadow-sm focus:outline-none focus:ring-indigo-500 focus:border-indigo-500" v-model="selectedMake" @change="getModels" :disabled="!selectedYear">
            <option value="">Select a make</option>
            <option v-for="make in makes" :key="make.id" :value="make">{{ make.make }}</option>
        </select>
        </div>

        <div class="mx-2">
        <select class="block w-full py-2 px-3 border border-gray-300 bg-white rounded-md shadow-sm focus:outline-none focus:ring-indigo-500 focus:border-indigo-500" v-model="selectedModel" @change="getTypes" :disabled="!selectedMake">
            <option value="">Select a model</option>
            <option v-for="model in models" :key="model.id" :value="model">{{ model.model }}</option>
        </select>
        </div>

        <div class="mx-2">
        <select class="block w-full py-2 px-3 border border-gray-300 bg-white rounded-md shadow-sm focus:outline-none focus:ring-indigo-500 focus:border-indigo-500" v-model="selectedType" :disabled="!selectedModel">
            <option value="">Select a type</option>
            <option v-for="type in types" :key="type.id" :value="type">{{ type.car_type }}</option>
        </select>
        </div>
    </div>
</template>

<script>
import axios from 'axios'

export default {
    data() {
        return {
            years: [],
            makes: [],
            models: [],
            types: [],
            selectedYear: '',
            selectedMake: '',
            selectedModel: '',
            selectedType: '',
        }
    },
    methods: {
        getYears() {

            axios.get('/years', { params: {} })
                .then(response => {
                    this.years = response.data
                })
        },
        getMakes() {
            axios.get(`/years/${this.selectedYear.year}/makes`, { params: {
                year: this.selectedYear.id
            }})
                .then(response => {
                    this.makes = response.data
                    this.models = []
                    this.types = []
                })
        },
        getModels() {
            axios.get(`/years/${this.selectedYear.year}/makes/${this.selectedMake.make}/models`, { params: {
                    make: this.selectedMake.make
                }})
                .then(response => {
                    this.models = response.data
                    this.types = []
                })
        },
        getTypes() {
            axios.get(`/years/${this.selectedYear.year}/makes/${this.selectedMake.make}/models/${this.selectedModel.model}/types`, { params: {
                    model: this.selectedModel.model
                }})
                .then(response => {
                    this.types = response.data
                })
            console.log('selectedModel: ', this.selectedModel)
        },
    },
    created() {
        this.getYears()
    },
}
</script>

<style></style>