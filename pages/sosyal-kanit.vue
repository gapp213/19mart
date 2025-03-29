<template>
    <div class="flex flex-col gap-2.5 md:gap-4 justify-center max-w-[1440px] w-full mx-auto">
        <div class="flex gap-2 md:gap-2.5 px-2.5 pt-2.5 md:px-5 md:pt-5 ">
            <div class="bg-red-200 rounded-md px-3.5 py-2.5 text-red-600 text-sm font-medium leading-relaxed">
                Herhangi bir güvenlik gücüne ait orantısız güç kullanımı, haksız yere gözaltına alınma, tutuklanma,
                işkence, kötü muamele, mal varlığına el konulması durumlarının kayıt altına alınması ve kamuoyu ile paylaşılması
                amacıyla oluşturulmuştur. Hiçbir güvenlik kurumu kendi milletine silah doğrultamaz ve güç kullanamaz. Tüm kanıtlanmış veriler avukatlar tarafından incelenmeli
                ve hukuki süreç başlatılmalıdır. Belki bugün değil ama yarın adalet yerini bulduğunda burdaki kanıtlar bu süreçte önemli bir rol oynayacaktır.
            </div>
        </div>
        <div class="flex gap-2 md:gap-2.5 px-2.5 md:px-5 mt-1 text-sm">
            <!-- Filters -->
            <div class="flex flex-1 max-w-[200px]">
                <div class="flex w-full border rounded-md border-gray-300">
                    <select v-model="activeSehir"
                        class="px-2.5 py-1.5 md:px-3.5 md:py-2 border border-r-[10px] flex-1 bg-white border-white focus:outline-red-600 rounded-sm transition duration-200">
                        <option v-for="sehir in sehirler" :key="sehir" :value="sehir">{{ sehir }}
                        </option>
                    </select>
                </div>
            </div>

            <!-- Search -->
            <input type="text" v-model="search"
                class="px-3 py-1.5 md:px-3.5 md:py-2 min-w-[130px] border bg-white border-gray-300 focus:outline-red-600 rounded-sm flex-1 transition duration-200"
                placeholder="Ara...">

            <!-- Add Button -->
            <button @click="showModal = true" class="px-2.5 py-1.5 md:px-3.5 md:py-2 cursor-pointer rounded-sm transition-colors duration-200 text-base flex items-center justify-center text-white bg-green-500 hover:bg-green-600 ">
                <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24"><path fill="none" stroke="currentColor" stroke-linecap="round" stroke-width="2" d="M12 20v-8m0 0V4m0 8h8m-8 0H4"/></svg></button>
        </div>

        <!-- Content -->
        <div class="md:px-5 xtable mb-6 overflow-y-auto"> 
            <table class="min-w-full divide-y divide-gray-200 md:shadow-sm md:rounded-md">
                <thead class="bg-gray-50">
                    <tr class="text-xs md:text-sm text-left font-medium text-white tracking-wider">
                        <th class="font-medium bg-red-600 w-[200px]">AD SOYAD</th>
                        <th class="font-medium bg-red-600 min-w-[115px]">SİCİL/KİMLİK</th>
                        <th class="font-medium bg-red-600">SORUN/OLAY</th>
                        <th class="font-medium bg-red-600 w-[120px] min-w-[110px]">KANIT MEDYA</th>
                        <th class="font-medium bg-red-600 w-[110px]">ŞEHİR</th>
                        <th class="font-medium bg-red-600 w-[150px]">TARİH</th>
                    </tr>
                </thead>
                <tbody class="bg-white divide-y divide-gray-200 text-sm md:text-sm">
                    <tr v-for="(item, id) in filteredData" :key="id" class="duration-200 hover:bg-gray-50 text-gray-700">
                        <td class="whitespace-nowrap">{{ item['Ad Soyad'] }}</td>
                        <td class="whitespace-nowrap">{{ item['Sicil yada Kimlik'] }}</td>
                        <td class="whitespace-nowrap">{{ item['Sebep'] }}</td>
                        <td class="whitespace-nowrap">
                            <a class="flex justify-center hover:fill-red-600 transition duration-200" @click.prevent="openKanitModal(item['Kanıt'])" href="#">
                                <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24"><path d="m7.5 19.5l4-2.5l-4-2.5zM13 7.75h4v-1.5h-4zM3 23q-.825 0-1.412-.587T1 21v-8q0-.825.588-1.412T3 11h12q.825 0 1.413.588T17 13v8q0 .825-.587 1.413T15 23zm15.5-9v-1q0-.95-.462-1.75T16.8 10h.2q1.25 0 2.125-.875T20 7t-.875-2.125T17 4h-1.25v1.5H17q.625 0 1.063.438T18.5 7t-.437 1.063T17 8.5h-1.25V10q-.2 0-.375-.25T15 9.5h-.75v-1H13q-.625 0-1.062-.437T11.5 7t.438-1.062T13 5.5h1.25V4H13q-1.25 0-2.125.875T10 7q0 .8.375 1.438T11.35 9.5H7V3q0-.825.588-1.412T9 1h12q.825 0 1.413.588T23 3v9q0 .825-.587 1.413T21 14z"/></svg>                            </a>
                        </td>
                        <td class="whitespace-nowrap">{{ item['Şehir'] }}</td>
                        <td class="whitespace-nowrap">{{ item['Tarih'] }}</td>
                    </tr>
                </tbody>
            </table>
        </div>
    </div>

    <!-- Modal Add -->
    <div v-if="showModal" class="fixed inset-0 bg-[rgba(0,0,0,.25)] flex items-center justify-center" @click.self="showModal = false">
        <div class="bg-white px-6 py-5 m-6 rounded-md w-full max-w-2xl shadow-xl relative">
            <button @click="showModal = false" class="cursor-pointer absolute right-0 top-0 text-2xl font-bold rounded-full bg-white shadow-md p-2.5 transform translate-x-1/2 -translate-y-1/2">
                <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24"><path fill="none" stroke="red" stroke-dasharray="12" stroke-dashoffset="12" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 12l7 7M12 12l-7 -7M12 12l-7 7M12 12l7 -7"><animate fill="freeze" attributeName="stroke-dashoffset" dur="0.3s" values="12;0"/></path></svg>
            </button>
            
            <h2 class="text-2xl mb-4">Kanıt Nasıl Eklenir?</h2>
            <div class="text-base flex flex-col gap-2 leading-relaxed">
                <ul class="pl-5 list-disc">
                    <li class="pl-1.5">Kanıt eklemek için <b>Kanıt Ekleme Formunu</b> doldurun.</li>
                    <li class="pl-1.5"><b>Resim/Video</b> yükleyerek kanıtlarınızı ekleyebilirsiniz.</li>
                    <li class="pl-1.5">Sanığa dair <b>İsim/Sicil/TC</b> bilmiyorsanız, detaylı görüntü aldığından emin olun.</li>
                    <li class="pl-1.5">Dosya yükleme işlemi olduğunda google hesabınız ile oturum açmanız gerekse bile size ait hiçbir veri toplanmıyor.</li>
                    <li class="pl-1.5">Listeye eklenmesi 2-3 saat kadar sürebilir.</li>
                </ul>

                Buraki tüm veriler topluma açıktır ve herkes tarafından görüntülenebilir.
            </div>

            <div class="flex justify-end mt-5">
                <a href="https://docs.google.com/forms/d/e/1FAIpQLSdhlWvtt1sA-nkElEv5NQuB0N_QddZ5m4TQWzRkNX3RMzH2kg/viewform?usp=header" target="_blank" class="flex items-center px-3 h-10 md:px-3.5 md:py-2 bg-blue-500 text-white rounded hover:bg-blue-600 cursor-pointer">Kanıt Ekleme Formuna Git</a>
            </div>
        </div>
    </div>

    <!-- Modal Kanıt -->
    <div v-if="showKanitModal" class="fixed inset-0 bg-[rgba(0,0,0,.25)] flex items-center justify-center" @click.self="showKanitModal = false">
        <div class="bg-white p-3 m-6 rounded-md w-full max-w-5xl shadow-xl relative">
            <button @click="showKanitModal = false" class="cursor-pointer absolute right-0 top-0 text-2xl font-bold rounded-full bg-white shadow-md p-2.5 transform translate-x-1/2 -translate-y-1/2">
                <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24"><path fill="none" stroke="red" stroke-dasharray="12" stroke-dashoffset="12" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 12l7 7M12 12l-7 -7M12 12l-7 7M12 12l7 -7"><animate fill="freeze" attributeName="stroke-dashoffset" dur="0.3s" values="12;0"/></path></svg>
            </button>

            <div class="max-h-[90vh] overflow-auto flex gap-4 flex-row flex-wrap">
                <template v-for="(link, index) in getLinks(modalKanitLink)">
                    <iframe v-if="link.search('drive.google') !== -1" :src="'https://drive.google.com/file/d/'+ getDriveFileId(modalKanitLink) +'/preview'" class="w-full h-[800px] max-h-[83vh] rounded-md grow shrink basis-[48%]"></iframe>
                    <div v-else class="grow shrink basis-[100%]">
                        {{ index + 1 }}. Detaylar için <a :href="modalKanitLink" target="_blank" class="text-blue-500 underline">buraya</a> tıklayın.
                    </div>
                </template>
            </div>
        </div>
    </div>
</template>

<script setup>
import { ref, computed, watch } from 'vue';
import debounce from 'lodash.debounce';
import rawCSV from '../data/kanit.csv?raw';
import Papa from 'papaparse';

useSeoMeta({
  title: 'Türkiye Cumhuriyeti Sosyal Kanıt Listesi',
  description: 'Türkiye Cumhuriyeti Sosyal Kanıt Listesi',
})

const csvData = Papa.parse(rawCSV, { header: true, skipEmptyLines: true });
const data = csvData.data

const sehirler = ['Tümü', ...new Set(data.map(item => item['Şehir']))];
const activeSehir = ref('Tümü');
const search = ref('');

const debouncedSearch = ref('');
watch(search, (newVal) => debounce((value) => debouncedSearch.value = value, 400)(newVal));

const normalizeString = (str) => str.normalize('NFD').replace(/[\u0300-\u036f]/g, '').toLowerCase();

const filteredData = computed(() => {
    let filtered = data;
    if (activeSehir.value !== 'Tümü') {
        filtered = filtered.filter(item => item['Şehir'] === activeSehir.value);
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

const getDriveFileId = (url) => {
    const match = url.match(/id=([a-zA-Z0-9_-]+)/);
    return match ? match[1] : null;
}
const getLinks = (links) => {
    return links.split(',').map(link => link.trim());
}

// Modal
const showModal = ref(false);
const showKanitModal = ref(false);
const modalKanitLink = ref('');
const openKanitModal = (link) => {
    modalKanitLink.value = link;
    showKanitModal.value = true;
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