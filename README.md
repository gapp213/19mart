# Nuxt Minimal Starter

Build almaya gerek yok Cloudflare üzerinde otomatik dağıtım yapılıyor. 

Look at the [Nuxt documentation](https://nuxt.com/docs/getting-started/introduction) to learn more.

## Setup

Make sure to install dependencies:

```bash
# yarn
yarn install
```

## Development Server

Start the development server on `http://localhost:3000`:

```bash
# yarn
yarn dev
```

## Production

Build the application for production:

```bash
# static generate
yarn generate
```

Locally preview production build:

```bash
# yarn
yarn preview
```

Check out the [deployment documentation](https://nuxt.com/docs/getting-started/deployment) for more information.

## kanit.csv Ekleme

`data/kanit.csv` dosyasına ekleme yapmak için aşağıdaki formatı kullanın:

```csv
Ad Soyad,Sicil yada Kimlik,Kanıt,Şehir,Tarih,Sebep
John Doe,123456789,Google Drive Public Link (Video/Image),İstanbul,2023-10-01,Sebep açıklaması
```

## boykot.csv Ekleme

`data/boykot.csv` dosyasına ekleme yapmak için aşağıdaki formatı kullanın:

```csv
isim,kategori,sebep,sehir,konum
John Doe,Ulaşım,Sebep açıklaması,İstanbul,40.93784734591586;29.124772782482356
John Doe,Ulaşım,Sebep açıklaması,İstanbul,https://www.google.com/maps/embed?....
```
