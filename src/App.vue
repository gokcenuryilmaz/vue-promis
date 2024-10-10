<template>
  <div id="app">
    <h1>Albums</h1>
    <ul v-if="albums.length">
      <li v-for="album in albums" :key="album.id">
        {{ album.title }}
      </li>
    </ul>
    <p v-else>Loading albums...</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      albums: [], // Album verilerini tutacak dizi
    };
  },
  created() {
    this.fetchAlbums(); // Bileşen oluşturulduğunda verileri çek
  },
  methods: {
    // Album verilerini çeken metod
    fetchAlbums() {
      // JSONPlaceholder'dan veri çeken bir Promise
      const fetchData = () => {
        return new Promise((resolve, reject) => {
          fetch("https://jsonplaceholder.typicode.com/albums")
            .then((response) => {
              if (!response.ok) {
                throw new Error("Network response was not ok");
              }
              return response.json();
            })
            .then((data) => resolve(data))
            .catch((error) => reject(error));
        });
      };

      // Promise'i kullanarak verileri al
      fetchData()
        .then((data) => {
          this.albums = data; // Alınan veriyi albümler dizisine ata
        })
        .catch((error) => {
          console.error("Error fetching albums:", error); // Hata durumunda konsola yaz
        });
    },
  },
};
</script>

<style>
#app {
  text-align: center; /* Metni ortalar */
  font-family: Arial, sans-serif; /* Yazı tipi */
  background-color: #f0f0f0; /* Arka plan rengi */
  padding: 20px; /* İç boşluk */
  border-radius: 8px; /* Köşe yuvarlama */
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1); /* Gölge efekti */
  max-width: 600px; /* Maksimum genişlik */
  margin: 20px auto; /* Merkeze alma */
}

h1 {
  color: #333; /* Başlık rengi */
  margin-bottom: 20px; /* Başlık ile altındaki öğe arasındaki boşluk */
}

ul {
  list-style-type: none; /* Liste işaretlerini kaldır */
  padding: 0; /* İç boşluğu sıfırla */
}

li {
  background-color: #fff; /* Liste öğelerinin arka plan rengi */
  margin: 10px 0; /* Öğeler arasındaki boşluk */
  padding: 15px; /* İç boşluk */
  border-radius: 5px; /* Köşe yuvarlama */
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1); /* Gölge efekti */
  transition: transform 0.2s; /* Geçiş efekti */
}

li:hover {
  transform: scale(1.02); /* Üzerine gelindiğinde büyüme efekti */
  cursor: pointer; /* İşaretçi şekli */
}

p {
  color: #777; /* "Loading albums..." mesajının rengi */
  font-style: italic; /* İtalik yazı stili */
}
</style>

