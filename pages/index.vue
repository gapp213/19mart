<template>
    <div class="flex flex-col gap-2.5 md:gap-4 justify-center max-w-[1440px] w-full mx-auto">
        <div class="flex gap-2 md:gap-2.5 px-2.5 pt-2.5 md:px-5 md:pt-5 text-sm">
            <!-- Filters -->
            <div class="hidden lg:flex gap-1 md:gap-1 text-sm">
                <button v-for="kategori in kategoriler" :key="kategori" @click="activeKategori = kategori"
                    class="px-2.5 py-2 md:px-3 md:py-2 cursor-pointer rounded-sm border transition-colors duration-200"
                    :class="{
                        'bg-white hover:bg-gray-100 border-gray-300': activeKategori !== kategori,
                        'bg-red-600 hover:bg-red-600 text-white border-red-600': activeKategori === kategori
                    }">
                    {{ kategori }}
                </button>
            </div>

            <!-- Filters Mobile -->
            <div class="lg:hidden flex flex-1 max-w-[200px]">
                <div class="flex w-full border rounded-md border-gray-300">
                    <select v-model="activeKategori"
                        class="px-2.5 py-1.5 md:px-3.5 md:py-2 border border-r-[10px] flex-1 bg-white border-white focus:outline-red-600 rounded-sm transition duration-200">
                        <option v-for="kategori in kategoriler" :key="kategori" :value="kategori">{{ kategori }}
                        </option>
                    </select>
                </div>
            </div>

            <!-- Search -->
            <input type="text" v-model="search"
                class="px-3 py-1.5 md:px-3.5 md:py-2 border min-w-[130px] bg-white border-gray-300 focus:outline-red-600 rounded-sm flex-1 transition duration-200"
                placeholder="Ara...">

            <!-- Add Button -->
            <NuxtLink to="/add-form"
                class="px-2.5 py-1.5 md:px-3.5 md:py-2 cursor-pointer rounded-sm transition-colors duration-200 text-base flex items-center justify-center text-white bg-green-500 hover:bg-green-600 ">
                <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24">
                    <path fill="none" stroke="currentColor" stroke-linecap="round" stroke-width="2"
                        d="M12 20v-8m0 0V4m0 8h8m-8 0H4" />
                </svg>
            </NuxtLink>
        </div>

        <!-- Content -->
        <div class="md:px-5 xtable mb-6 overflow-y-auto">
            <table class="min-w-full divide-y divide-gray-200 md:shadow-sm md:rounded-md">
                <thead class="bg-gray-50">
                    <tr class="text-xs md:text-sm text-left font-medium text-white tracking-wider">
                        <th class="font-medium bg-red-600">İSİM</th>
                        <th class="font-medium bg-red-600 w-[130px] min-w-[95px]">KATEGORİ</th>
                        <th class="font-medium bg-red-600">AÇIKLAMA</th>
                        <th class="font-medium bg-red-600 w-[160px]">ŞEHİR</th>
                        <th class="font-medium bg-red-600 w-[80px]">KONUM</th>
                    </tr>
                </thead>
                <tbody class="bg-white divide-y divide-gray-200 text-sm md:text-sm">
                    <tr v-for="(item, id) in filteredData" :key="id"
                        class="duration-200 hover:bg-gray-50 text-gray-700">
                        <td class="whitespace-nowrap">{{ item.isim }}</td>
                        <td class="whitespace-nowrap">{{ item.kategori }}</td>
                        <td class="whitespace-nowrap">{{ item.sebep }}</td>
                        <td class="whitespace-nowrap">{{ item.sehir }}</td>
                        <td>
                            <a v-if="item.konum" class="flex justify-center hover:fill-red-600 transition duration-200"
                                @click.prevent="openModal(item.konum)"
                                href="#">
                                <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24">
                                    <path
                                        d="M12 3a7 7 0 0 0-7 7c0 2.862 1.782 5.623 3.738 7.762A26 26 0 0 0 12 20.758q.262-.201.615-.49a26 26 0 0 0 2.647-2.504C17.218 15.623 19 12.863 19 10a7 7 0 0 0-7-7m0 20.214l-.567-.39l-.003-.002l-.006-.005l-.02-.014l-.075-.053l-.27-.197a28 28 0 0 1-3.797-3.44C5.218 16.875 3 13.636 3 9.999a9 9 0 0 1 18 0c0 3.637-2.218 6.877-4.262 9.112a28 28 0 0 1-3.796 3.44a17 17 0 0 1-.345.251l-.021.014l-.006.005l-.002.001zM12 8a2 2 0 1 0 0 4a2 2 0 0 0 0-4m-4 2a4 4 0 1 1 8 0a4 4 0 0 1-8 0" />
                                </svg>
                            </a>
                        </td>
                    </tr>
                </tbody>
            </table>
        </div>
    </div>

    <!-- Modal  -->
    <div v-if="showModal" class="fixed inset-0 bg-[rgba(0,0,0,.25)] flex items-center justify-center"
        @click.self="showModal = false">
        <div class="bg-white p-3 m-6 rounded-md w-full max-w-5xl shadow-xl relative">
            <button @click="showModal = false"
                class="cursor-pointer absolute right-0 top-0 text-2xl font-bold rounded-full bg-white shadow-md p-2.5 transform translate-x-1/2 -translate-y-1/2">
                <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24">
                    <path fill="none" stroke="red" stroke-dasharray="12" stroke-dashoffset="12" stroke-linecap="round"
                        stroke-linejoin="round" stroke-width="2" d="M12 12l7 7M12 12l-7 -7M12 12l-7 7M12 12l7 -7">
                        <animate fill="freeze" attributeName="stroke-dashoffset" dur="0.3s" values="12;0" />
                    </path>
                </svg>
            </button>
            
            <iframe :src="getMapLink" class="w-full h-[800px] max-h-[83vh] rounded-md"></iframe>
        </div>
    </div>
</template>

<script setup>
import { ref, computed, watch } from 'vue';
import debounce from 'lodash.debounce';
import rawCSV from '../data/boykot.csv?raw';
import Papa from 'papaparse';

const csvData = Papa.parse(rawCSV, { header: true, skipEmptyLines: true });
const data = csvData.data

const kategoriler = ['Tümü', ...new Set(data.map(item => item.kategori))];
const activeKategori = ref('Tümü');
const search = ref('');

const debouncedSearch = ref('');
watch(search, (newVal) => debounce((value) => debouncedSearch.value = value, 400)(newVal));

const normalizeString = (str) => str.normalize('NFD').replace(/[\u0300-\u036f]/g, '').toLowerCase();

const filteredData = computed(() => {
    let filtered = data;
    if (activeKategori.value !== 'Tümü') {
        filtered = filtered.filter(item => item.kategori === activeKategori.value);
    }
    if (debouncedSearch.value) {
        const normalizedSearch = normalizeString(debouncedSearch.value);
        filtered = filtered.filter(item =>
            Object.values(item).some(val =>
                normalizeString(String(val)).includes(normalizedSearch)
            )
        );
    }
    return filtered;
});

// Modal
const showModal = ref(false);
const modalLink = ref('');
const getMapLink = computed(() => {
    if (modalLink.value.search('google.com') !== -1) {
        return modalLink.value;
    }
    return `https://www.google.com/maps?q=${modalLink.value.replace(';', ',')}&output=embed`
});
const openModal = (link) => {
    modalLink.value = link;
    showModal.value = true;
};
</script>

<style lang="scss">
.xtable {
    thead {
        th {
            padding: .65rem 1rem;
            padding-right: 0;

            &:first-child {
                @media (min-width: 768px) {
                    border-top-left-radius: 0.375rem;
                }
            }

            &:last-child {
                padding-right: 1rem;

                @media (min-width: 768px) {
                    border-top-right-radius: 0.375rem;
                }
            }
        }
    }

    tbody {
        td {
            padding: .65rem 1rem;
            padding-right: 0;

            &:last-child {
                padding-right: 1rem;
            }
        }

        tr:last-child {
            td {
                &:first-child {
                    @media (min-width: 768px) {
                        border-bottom-left-radius: 0.375rem;
                    }
                }

                &:last-child {
                    @media (min-width: 768px) {
                        border-bottom-right-radius: 0.375rem;
                    }
                }
            }
        }
    }
}
</style>