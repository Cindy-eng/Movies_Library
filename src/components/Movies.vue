<script>
export default {
  data() {
    return {
      movies: [], // Lista de filmes
      loading: false, // Status de carregamento
      errorMessage: '', // Mensagem de erro
    };
  },
  methods: {
    async fetchMovies(query) {
      const API_URL = 'https://api.collectapi.com/imdb/imdbSearchByName';
      const API_KEY = 'apikey 3NxW9JhF6FbNjEZ7ftokk8:58O5ZWD6641TJonL1zqVWO';

      this.loading = true;
      this.errorMessage = '';
      this.movies = [];
      try {
        const response = await fetch(`${API_URL}?query=${encodeURIComponent(query)}`, {
          method: 'GET',
          headers: {
            Authorization: API_KEY,
          },
          credentials: 'omit',
        });

        if (!response.ok) {
          throw new Error('Falha ao carregar dados');
        }

        const data = await response.json();
        if (data.result) {
          this.movies = data.result;
        } else {
          this.errorMessage = 'Nenhum filme encontrado.';
        }
      } catch (error) {
        console.error('Erro ao carregar dados:', error);
        this.errorMessage = 'Erro ao carregar os filmes. Tente novamente mais tarde.';
      } finally {
        this.loading = false;
      }
    },
  },
  mounted() {
    this.fetchMovies('Avengers'); // Buscando filmes ao carregar a página
  },
};
</script>
<template>
  <div class="container-fluid p-4">
    <div class="card-header">
    <header class="text-center mb-5">
      <h1 class="display-4 fw-bold">Galeria de Filmes IMDB</h1>
      <p class="lead">Descubra filmes incríveis ao seu alcance.</p>
    </header>
    </div>
    <main>
      <div v-if="loading" class="text-center">
        <div class="spinner-border text-light" role="status">
          <span class="visually-hidden">Carregando...</span>
        </div>
      </div>
      <p v-if="errorMessage" class="text-center text-danger">{{ errorMessage }}</p>
      <div v-if="!loading && movies.length > 0" class="movies-list">
        <div
            v-for="movie in movies"
            :key="movie.imdbID"
            class="card-wrapper"
        >
          <div class="card">
            <a :href="'https://www.imdb.com/title/' + movie.imdbID" target="_blank">
              <img
                  :src="movie.Poster && movie.Poster !== 'N/A' ? movie.Poster : 'https://via.placeholder.com/150'"
                  class="card-img-top"
                  :alt="movie.Title || 'Título não disponível'"
              />
            </a>
            <div class="card-body">
              <h5 class="card-title">{{ movie.Title || 'Título não disponível' }}</h5>
            </div>
          </div>
        </div>


      </div>

    </main>
    <footer>
      <p>&copy; 2024 Galeria de Filmes IMDB. Todos os direitos reservados.</p>
      <p>Desenvolvido por <a href="https://www.seusite.com" target="_blank">Agílio Jamisse, Cindy Zacarias,	Grácio Kabongo, Harmónico Augusto</a></p>
    </footer>
  </div>
</template>

<style scoped>
body {
  background-color: #121212;
  color: #f0f0f0;
  font-family: 'Roboto', sans-serif;
}

header h1 {

  text-shadow: 2px 2px 4px #000;
}

.card {
  background: #1e1e2f;
  border: none;
  border-radius: 15px;
  overflow: hidden;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
  width: 100%;
  max-width: 280px; /* Ajusta a largura máxima da carta */
  margin: 0 auto;
}

.card:hover {
  transform: translateY(-10px);
  box-shadow: 0 8px 15px rgba(0, 255, 255, 0.3);
}

.card img {
  height: 200px; /* Ajustando a altura das imagens */
  width: 100%; /* Garantindo que a imagem ocupe a largura total da carta */
  object-fit: cover;
}


.card-title {
  font-size: 1.2rem;
  color: #00bcd4;
  margin-top: 10px;
  text-align: center;
}
header {
  text-align: center;
  margin-bottom: 50px;
}

header h1 {

  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.7);
}

header p {
  color: #e0e0e0;
  font-size: 1.1rem;
}
.card-header {
  background: #1e1e2f; /* Azul escuro */
  padding: 20px 30px; /* Ajustando o padding para um tamanho médio */
  border-radius: 15px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.6);
  display: flex;
  flex-direction: column;
  align-items: center;
  margin: 0 auto; /* Centraliza o card na tela */
  width: 70%; /* Largura do cartão é 70% */
  max-width: 600px; /* Tamanho máximo do cartão */
  transition: transform 0.3s ease, box-shadow 0.3s ease;
  margin-bottom: 100px;
}
/* Header */
header {
  text-align: center;
  margin: 0;
  color: white;
}

header h1 {
  color: #ffffff;
  font-size: 2.5rem;
  font-weight: bold;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.6);
}


/* Efeito de movimento no cartão */
.card-header:hover {
  transform: translateY(-1px);
  box-shadow: 0 8px 15px rgba(0, 255, 255, 0.3);
}


.movies-list {
  display: flex;
  flex-wrap: wrap;
  gap: 1rem;
  justify-content: center;
}

.card-wrapper {
  width: 100%;
  max-width: 320px;
}
footer {
  background-color: #1e1e2f;
  color: #ffffff;
  text-align: center;
  padding: 20px;
  font-size: 1rem;
  box-shadow: 0 -4px 8px rgba(0, 0, 0, 0.6);
  margin-top: 50px;
}

footer a {
  color: #00bcd4;
  text-decoration: none;
}

footer a:hover {
  text-decoration: underline;
}

@media (max-width: 768px) {
  .card img {
    height: 200px;
  }
  .card-header {
    width: 90%; /* Maior largura em telas menores */
    padding: 15px 20px;
  }

  header h1 {
    font-size: 2rem; /* Tamanho do título reduzido em telas pequenas */
  }

  header p {
    font-size: 1rem; /* Tamanho da descrição ajustado */
  }

}
</style>