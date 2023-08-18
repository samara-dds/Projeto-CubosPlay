# Projeto-CubosPlay

Durante essa semana foi desenvolvido uma aplição de um site de filmes. Nele você pode pesquisar qualquuer filme, se ouver o filme que vc procura ele vai te mostrar, tambem temos o catalogo de filmes, nele você encontra os filmes já disponiveis no site, tambem tem o "Filme do Dia", e ele te mostra o filme que está em alta no dia.

##Imagens FrontEnd##

![Site]![image](https://github.com/samara-dds/Projeto-CubosPlay/assets/130112139/4f18565e-c5bc-41a9-8f06-f7385c6708b7)


![Codigo]

<h3>Importações</h3>
import React, { useState, useEffect } from 'react';
import React, { useState } from 'react';
import logoDark from './assets/logo-dark.png';
import { ReactComponent as LightMode } from './assets/light-mode.svg';
import { ReactComponent as ArrowLeftDark } from './assets/arrow-left-dark.svg';
@@ -9,97 +9,51 @@ import { ReactComponent as CloseDark } from './assets/close-dark.svg';
import './css/global.css';
import './css/style.css';
import BarraDePesquisa from './Components/BarraDePesquisa';
import { movies, highlightMovie, playMovie } from './services/movies';


<h3>Lógica do Site</h3>
function App() {
  const [paginaAtual, setPaginaAtual] = useState(0);

  const totalFilmesDisplay = 6;
  const indexInicial = paginaAtual * totalFilmesDisplay;
  const filmeExibição = movies.slice(indexInicial, indexInicial + totalFilmesDisplay);

  const handlePageChange = (newPage) => {

    setPaginaAtual(newPage);
  const indexInicial = paginaAtual;
  const trocarDisplay = movies.results.slice(indexInicial, indexInicial + totalFilmesDisplay);

  const proximaPagina = () => {
    setPaginaAtual((page) => {
      if (page + totalFilmesDisplay >= movies.results.length) {
        return 0;
      } else {
        return page + totalFilmesDisplay;
      }
    });
  };

  const paginaAnterior = () => {
    setPaginaAtual((page) => {
      if (page === 0) {
        return movies.results.length - totalFilmesDisplay;
      } else {
        return page - totalFilmesDisplay;
      }
    });
  };

  const [filmesCorrespondentes, setFilmesCorrespondentes] = useState([]);

  const [searchValue, setSearchValue] = useState('');


  const handleFilmePesquisa = (searchTerm) => {
    const filmesEncontrados = movies.results.filter((movie) =>
      movie.title.toLowerCase().includes(searchTerm)
    );

    setSearchValue(searchTerm);
    setFilmesCorrespondentes(filmesEncontrados);
  };

  

  <h3>Conteúdo do Site</h3>
   return (
    <>
@@ -109,17 +63,16 @@ function App() {
          <h1 className="header__title">CUBOS FLIX</h1>
        </div>
        <div className="header__container-right">

          <BarraDePesquisa movies={movies} onSearch={(searchValue) => console.log(searchValue)} />
          <BarraDePesquisa movies={movies.results} onSearch={handleFilmePesquisa} />
          <LightMode className="btn-theme" />
        </div>
      </header>
      <div className="container size">
        <div className="movies-container">
          <ArrowLeftDark className="btn-prev" onClick={() => setPaginaAtual(paginaAtual - 1)} />
          <ArrowLeftDark className="btn-prev" onClick={paginaAnterior} />
          <div className="movies">
            {filmeExibição.map((movie) => (
              <div key={movie.id} className="movie" style={{ backgroundImage: `url(${movie.poster})` }}>
            {trocarDisplay.map((movie) => (
              <div key={movie.id} className="movie" style={{ backgroundImage: `url(${movie.poster_path})` }}>
                <div className="movie__info">
                  <span className="movie__title" title={movie.title}>
                    {movie.title}
@@ -132,12 +85,14 @@ function App() {
              </div>
            ))}
          </div>
          <ArrowRightDark className="btn-next" onClick={() => setPaginaAtual(paginaAtual + 1)} />
          <ArrowRightDark className="btn-next" onClick={proximaPagina} />
        </div>
        <div className="highlight size">
          {highlightMovie && (
            <a className="highlight__video-link" href={`https://www.youtube.com/watch?v=${highlightMovie.trailerId}`} target="_blank" rel="noopener noreferrer">

            <a className="highlight__video-link" href={`https://www.youtube.com/watch?v=${playMovie.key}`} target="_blank" rel="noopener noreferrer">
              <div className="highlight__video">
                <img src={highlightMovie.backdrop_path} alt={highlightMovie.title} />
                <Play />
              </div>
            </a>
@@ -146,14 +101,18 @@ function App() {
            {highlightMovie && (
              <div className="highlight__title-rating">
                <h1 className="highlight__title">{highlightMovie.title}</h1>
                <span className="highlight__rating">{highlightMovie.rating}</span>
                <span className="highlight__rating">{highlightMovie.vote_average}</span>
              </div>
            )}
            <div className="highlight__genre-launch">
              <span className="highlight__genres">
                {highlightMovie.genre}</span> /
                {highlightMovie.genres.map((genre) => genre.name).join(', ')}</span> /
              <span className="highlight__launch">
                {highlightMovie.release_date}</span>
                {new Date(highlightMovie.release_date).toLocaleDateString('pt-BR', {
                  year: 'numeric',
                  month: 'long',
                  day: 'numeric',
                })}</span>
            </div>
            {highlightMovie && (
              <p className="highlight__description">
                {highlightMovie.overview}
              </p>
            )}
          </div>
        </div>
      </div >
      <div className="modal hidden">
        <div className="modal__body">
          <CloseDark className="modal__close" />
          <h3 className="modal__title">Titulo do filme</h3>
          <img className="modal__img" alt="modal__img" src={'https://image.tmdb.org/t/p/original/wTS3dS2DJiMFFgqKDz5fxMTri.jpg'} />
          <p className="modal__description">
            a long established fact that a reader will be distracted by the readable
            content of a page when looking at its layout. The point of using Lorem Ipsum
            is that it has a more-or-less normal distribution of letters, as opposed to
            using 'Content here, content here', making it look like readable English. Many
            desktop publishing packages and web page editors now use Lorem Ipsum as their
            default model text, and a search for 'lorem ipsum' will uncover many web sites
            still in their infancy. Various ve
          </p>
          <div className="modal__genre-average">
            <div className="modal__genres">
              <span className="modal__genre">Gênero1</span>
              <span className="modal__genre">Gênero2</span>
              <span className="modal__genre">Gênero3</span>
            </div>
            <div className="modal__average">1.1</div>
          </div>
        </div>
      </div>
    </>
  );
}
export default App;
