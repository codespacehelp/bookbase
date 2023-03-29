<script>
  import books from "./books.csv";
  console.log(books);

  let allGenresSet = new Set();
  for (const book of books) {
    let genres = book.genres.split(";");
    genres = genres.map((genre) => genre.trim());
    genres.forEach((genre) => allGenresSet.add(genre));
  }
  const allGenres = Array.from(allGenresSet);

  let activeBook;
  let activeGenre;

  let filteredBooks = books;
  //   if (activeGenre) {
  //     filteredBooks = books.filter((book) => book.genres.includes(activeGenre));
  //   }

  function setActiveBook(book) {
    activeBook = book;
  }

  function setActiveGenre(genre) {
    activeGenre = genre;
  }

  $: if (activeGenre) {
    filteredBooks = books.filter((book) => book.genres.includes(activeGenre));
  } else {
    filteredBooks = books;
  }
</script>

<h1>Books</h1>

<div class="genres">
  <button on:click={() => setActiveGenre(null)}>All</button>
  {#each allGenres as genre}
    <button on:click={() => setActiveGenre(genre)}>{genre}</button>
  {/each}
</div>

<div class="side-by-side">
  <table>
    <tr>
      <th>Title</th>
      <th>Author</th>
      <th>Published</th>
    </tr>
    {#each filteredBooks as book}
      <tr
        on:mouseenter={() => setActiveBook(book)}
        on:mouseleave={() => setActiveBook(null)}
      >
        <td><a href={book.link}>{book.title}</a></td>
        <td>{book.author}</td>
        <td>{book.published}</td>
      </tr>
    {/each}
  </table>

  <div class="book-details">
    {#if activeBook}
      <img src="/covers/{activeBook.id}.jpg" alt={activeBook.title} />
      <h2>{activeBook.title}</h2>
    {/if}
  </div>
</div>

<style>
  table {
    border-collapse: collapse;
    font-size: 0.8rem;
  }
  th,
  td {
    border-bottom: 1px solid #ccc;
    padding: 0.5rem;
    text-align: left;
  }
  .side-by-side {
    display: flex;
    gap: 1rem;
    flex-wrap: wrap;
    align-items: flex-start;
  }

  .book-details {
    margin-top: 2rem;
  }

  .genres {
    display: flex;
    flex-wrap: wrap;
    gap: 0.2rem 0.2rem;
  }
</style>
