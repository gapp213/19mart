<template>
    <div class="flex flex-col gap-4 my-3 md:my-5 px-4 mx-auto justify-center max-w-[660px] w-full">
        <!-- Header -->
        <div class="text-center">
            <h2 class="text-xl">Boykot Ekleme Formu</h2>
            <p class="text-sm mt-2">Ekleme işlemi ortalama 2 saat içinde yapılacaktır.</p>
        </div>

        <!-- Form -->
        <form @submit.prevent="submitForm" class="flex flex-col gap-4 mb-2.5">
            <div>
                <input type="text" id="isim" v-model="formData.isim" required
                    class="px-3.5 h-10 md:px-3.5 md:py-2 border bg-white border-gray-300 rounded w-full focus:outline-green-600 placeholder-gray-700"
                    placeholder="İsim" />
                <div class="text-xs pl-4 pt-1">Kişi, işletme vb. isim bilgisi.</div>
            </div>
            <div class="flex w-full border rounded-md border-gray-300">
                <select v-model="formData.kategori" required
                    class="px-2.5 h-10 md:px-3 md:py-2.5 border-0 border-r-[10px] border-transparent flex-1 bg-white focus:outline-green-600 rounded-sm transition duration-200 text-gray-700">
                    <option value="">Kategori</option>
                    <option v-for="kategori in kategoriler" :key="kategori" :value="kategori">{{ kategori }}</option>
                </select>
            </div>
            <input type="text" id="konum" v-model="formData.sebep" required
                class="px-3.5 h-10 md:px-3.5 md:py-2 border bg-white border-gray-300 rounded w-full focus:outline-green-600 placeholder-gray-700"
                placeholder="Sebep/Açıklama" />
            <div>
                <div class="flex w-full border rounded-md border-gray-300">
                    <select v-model="formData.sehir"
                        class="px-2.5 h-10 md:px-3 md:py-2.5 border-0 border-r-[10px] border-transparent flex-1 bg-white focus:outline-green-600 rounded-sm transition duration-200 text-gray-700">
                        <option value="">Şehir</option>
                        <option v-for="sehir in sehirler" :key="sehir" :value="sehir">{{ sehir }}</option>
                    </select>
                </div>
                <div class="text-xs pl-4 pt-1">İşletmenin, kişinin bulunduğu şehir, boş bırakabilirsiniz.</div>
            </div>                
            <div class="flex flex-col gap-1">
                <div class="flex gap-3 w-full">
                    <input type="text" id="konum" v-model="formData.konum"
                        class="px-3.5 h-10 md:px-3.5 md:py-2 border bg-white border-gray-300 rounded w-full focus:outline-green-600 placeholder-gray-700"
                        placeholder="Konum (Lat,Lng)" />
                    <button type="button" @click="showModal = true" class="px-3 border-none h-10 md:px-3.5 md:py-2 bg-orange-500 text-white rounded hover:bg-orange-600 cursor-pointer">?</button>
                </div>
                <div class="text-xs pl-4">Yerel işletmeler için konum bilgisi.</div>
            </div>
            <button type="submit" :disabled="isSubmitting"
                class="px-3 h-10 md:px-3.5 md:py-2 bg-blue-500 text-white rounded hover:bg-blue-600 cursor-pointer disabled:opacity-50">
                <span v-if="isSubmitting">Gönderiliyor...</span>
                <span v-else>Gönder</span>
            </button>
        </form>
    </div>

    <!-- Modal -->
    <div v-if="showModal" class="fixed inset-0 bg-[rgba(0,0,0,.25)] flex items-center justify-center" @click.self="showModal = false">
        <div class="bg-white px-6 py-5 m-6 rounded-md w-full max-w-2xl shadow-xl relative">
            <button @click="showModal = false" class="cursor-pointer absolute right-0 top-0 text-2xl font-bold rounded-full bg-white shadow-md p-2.5 transform translate-x-1/2 -translate-y-1/2">
                <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24"><path fill="none" stroke="red" stroke-dasharray="12" stroke-dashoffset="12" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 12l7 7M12 12l-7 -7M12 12l-7 7M12 12l7 -7"><animate fill="freeze" attributeName="stroke-dashoffset" dur="0.3s" values="12;0"/></path></svg>
            </button>
            
            <h2 class="text-2xl mb-4">Konum Bilgisi Nasıl Alınır?</h2>
            <div class="text-base flex flex-col gap-2">
                <p>Google Maps üzerinde konumu seçin ve sağ tıklayarak <b>Kordinatları Kopyala</b> seçeneğini seçin yada İşletmeye tıklayın ardından Paylaş'a ve Harita Yerleştirme tıklayın, HTML'yi kopyalayın</p>
                <p><b>Örnek</b>: 41.0082, 28.9784</p>
                <p><b>Örnek Embed</b>: https://www.google.com/maps/embed?pb=!1m14!1....</p>
            </div>

            <div class="flex justify-end mt-4">
                <a href="https://www.google.com/maps" target="_blank" class="flex items-center px-3 h-10 md:px-3.5 md:py-2 bg-blue-500 text-white rounded hover:bg-blue-600 cursor-pointer">Google Maps'i Aç</a>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            formData: {
                isim: '',
                kategori: '',
                sehir: '',
                sebep: '',
                konum: ''
            },
            kategoriler: ['Yemek & Kahve', 'Kitap & Alışveriş', 'AVM', 'Şans Oyunu', 'Ulaşım', 'TV & Radyo', 'Otomotiv', 'Fenomen', 'Ünlüler'],
            sehirler: [
                'Adana', 'Adıyaman', 'Afyonkarahisar', 'Ağrı', 'Aksaray', 'Amasya', 'Ankara', 'Antalya', 'Ardahan', 'Artvin',
                'Aydın', 'Balıkesir', 'Bartın', 'Batman', 'Bayburt', 'Bilecik', 'Bingöl', 'Bitlis', 'Bolu', 'Burdur',
                'Bursa', 'Çanakkale', 'Çankırı', 'Çorum', 'Denizli', 'Diyarbakır', 'Düzce', 'Edirne', 'Elazığ', 'Erzincan',
                'Erzurum', 'Eskişehir', 'Gaziantep', 'Giresun', 'Gümüşhane', 'Hakkari', 'Hatay', 'Iğdır', 'Isparta', 'İstanbul',
                'İzmir', 'Kahramanmaraş', 'Karabük', 'Karaman', 'Kars', 'Kastamonu', 'Kayseri', 'Kırıkkale', 'Kırklareli', 'Kırşehir',
                'Kilis', 'Kocaeli', 'Konya', 'Kütahya', 'Malatya', 'Manisa', 'Mardin', 'Mersin', 'Muğla', 'Muş',
                'Nevşehir', 'Niğde', 'Ordu', 'Osmaniye', 'Rize', 'Sakarya', 'Samsun', 'Siirt', 'Sinop', 'Sivas',
                'Şanlıurfa', 'Şırnak', 'Tekirdağ', 'Tokat', 'Trabzon', 'Tunceli', 'Uşak', 'Van', 'Yalova', 'Yozgat', 'Zonguldak'
            ],
            showModal: false,
            isSubmitting: false
        }
    },
    methods: {
        async submitForm() {
            this.isSubmitting = true;

            // Validate Lat,Lng
            const latLngPattern = /^\s*-?\d+(\.\d+)?\s*,\s*-?\d+(\.\d+)?\s*$/;
            if (this.formData.konum) {
                if (!latLngPattern.test(this.formData.konum)) {
                    const src = this.formData.konum.match(/src="([^"]+)"/);
                    if (! src) {
                        alert('Lütfen geçerli bir konum (Lat,Lng) yada Embed HTML kodu girin yada boş bırakın.');
                        this.isSubmitting = false;
                        return;
                    }

                    this.formData.konum = src[1];
                } else {
                    this.formData.konum =  this.formData.konum.replace(',', ';').replace(' ', '')
                }
            }
          
            // Submit form
            fetch('https://docs.google.com/forms/u/0/d/e/1FAIpQLSeTXYTUQBS8KxqH5DxWSCx_Q8h2lFu9cimUpyOlZs8D_sWXBQ/formResponse', {
                method: 'post',
                mode: 'no-cors',
                headers: {
                    "content-type": "application/x-www-form-urlencoded",
                    "cache-control": "no-cache",
                    "accept": "text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7",
                },
                body: [
                    'entry.1419465415=' + this.formData.isim,
                    'entry.1662974150=' + this.formData.kategori,
                    'entry.1159660803=' + this.formData.sebep,
                    'entry.1263833211=' + this.formData.sehir,
                    'entry.1875015140=' + this.formData.konum,
                ].join('&'),
            }).then(() => {
                alert('Kaydınız alındı, en kısa sürede eklenecektir.');
                this.isSubmitting = false;
            }).catch(() => {
                alert('Bir hata oluştu, lütfen tekrar deneyin.');
                this.isSubmitting = false;
            });
        }
    }
}
</script>