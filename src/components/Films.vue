<template>
<div class="flex mx-auto justify-center gap-2 max-w-xs md:max-w-lg">
  <input type="file" accept=".zip" @change="handleFileUpload" class="file-input file-input-primary file-input-sm text-base-content rounded-2xl" />
  <!-- Export -->
  <div v-if="months.length" tabindex="0" role="button" @click="handleDownload" id="download-btn" class="btn btn-sm rounded-2xl btn-primary shadow-sm uppercase font-bold tracking-widest">
    <span>
      <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" class="size-4">
        <path stroke-linecap="round" stroke-linejoin="round" d="m2.25 15.75 5.159-5.159a2.25 2.25 0 0 1 3.182 0l5.159 5.159m-1.5-1.5 1.409-1.409a2.25 2.25 0 0 1 3.182 0l2.909 2.909m-18 3.75h16.5a1.5 1.5 0 0 0 1.5-1.5V6a1.5 1.5 0 0 0-1.5-1.5H3.75A1.5 1.5 0 0 0 2.25 6v12a1.5 1.5 0 0 0 1.5 1.5Zm10.5-11.25h.008v.008h-.008V8.25Zm.375 0a.375.375 0 1 1-.75 0 .375.375 0 0 1 .75 0Z" />
      </svg>
    </span>Save
  </div>
</div>
    
<div class="flex mx-auto justify-center items-center gap-2 w-auto max-w-xs md:max-w-lg">
  <!-- Month Select -->
  <div v-if="months.length" class="w-45 h-auto border-base-content/20 rounded-sm shadow-sm">
    <select 
      id="month-select" 
      v-model="selectedMonth" 
      class="select select-sm bg-base-100 border-base-content/20 text-base-content text-xs rounded-2xl uppercase font-bold tracking-widest max-w-xs"
    >
      <option class="capitalize text-xs bg-base-100 text-base-content font-semibold"
        v-for="monthYear in months" 
        :key="monthYear" 
        :value="monthYear"
      >
        {{ monthYear }}
      </option>
    </select>
  </div>

  <!-- Theme Select -->
  <div v-if="months.length" class="dropdown py-4">
    <div tabindex="0" role="button" class="btn bg-base-100 border-base-content/20 rounded-2xl btn-sm uppercase font-bold tracking-widest">
      <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" class="size-3">
        <path fill-rule="evenodd" d="M20.599 1.5c-.376 0-.743.111-1.055.32l-5.08 3.385a18.747 18.747 0 0 0-3.471 2.987 10.04 10.04 0 0 1 4.815 4.815 18.748 18.748 0 0 0 2.987-3.472l3.386-5.079A1.902 1.902 0 0 0 20.599 1.5Zm-8.3 14.025a18.76 18.76 0 0 0 1.896-1.207 8.026 8.026 0 0 0-4.513-4.513A18.75 18.75 0 0 0 8.475 11.7l-.278.5a5.26 5.26 0 0 1 3.601 3.602l.502-.278ZM6.75 13.5A3.75 3.75 0 0 0 3 17.25a1.5 1.5 0 0 1-1.601 1.497.75.75 0 0 0-.7 1.123 5.25 5.25 0 0 0 9.8-2.62 3.75 3.75 0 0 0-3.75-3.75Z" clip-rule="evenodd" />
      </svg> Theme
      <svg width="12px" height="12px" class="inline-block h-2 w-2 fill-current opacity-60" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 2048 2048"><path d="M1799 349l242 241-1017 1017L7 590l242-241 775 775 775-775z"></path></svg>
    </div>
    <ul tabindex="0" class="dropdown-content text-base-content bg-base-300 border border-base-content/20 rounded-sm z-50 w-52 my-2 p-2 shadow-2xl">
      <template v-for="(list, group) in themes" :key="group">
        <li><div class="divider divider-start text-xs px-3">{{ group }}</div></li>
        <li v-for="theme in list" :key="theme">
          <input
            type="radio"
            name="theme-dropdown"
            class="theme-controller w-full btn btn-sm btn-block btn-ghost justify-start rounded-sm"
            :aria-label="theme[0].toUpperCase() + theme.slice(1)"
            :value="theme" />
        </li>
      </template>
    </ul>
  </div>
  
  <!-- Options -->
  <div v-if="months.length" class="dropdown">
    <div tabindex="0" role="button" class="btn btn-sm rounded-2xl border-base-content/20 uppercase tracking-widest font-bold bg-base-100">
      <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" class="size-4">
        <path d="M18.75 12.75h1.5a.75.75 0 0 0 0-1.5h-1.5a.75.75 0 0 0 0 1.5ZM12 6a.75.75 0 0 1 .75-.75h7.5a.75.75 0 0 1 0 1.5h-7.5A.75.75 0 0 1 12 6ZM12 18a.75.75 0 0 1 .75-.75h7.5a.75.75 0 0 1 0 1.5h-7.5A.75.75 0 0 1 12 18ZM3.75 6.75h1.5a.75.75 0 1 0 0-1.5h-1.5a.75.75 0 0 0 0 1.5ZM5.25 18.75h-1.5a.75.75 0 0 1 0-1.5h1.5a.75.75 0 0 1 0 1.5ZM3 12a.75.75 0 0 1 .75-.75h7.5a.75.75 0 0 1 0 1.5h-7.5A.75.75 0 0 1 3 12ZM9 3.75a2.25 2.25 0 1 0 0 4.5 2.25 2.25 0 0 0 0-4.5ZM12.75 12a2.25 2.25 0 1 1 4.5 0 2.25 2.25 0 0 1-4.5 0ZM9 15.75a2.25 2.25 0 1 0 0 4.5 2.25 2.25 0 0 0 0-4.5Z" />
      </svg> Options
      <svg width="12px" height="12px" class="inline-block h-2 w-2 fill-current opacity-60" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 2048 2048"><path d="M1799 349l242 241-1017 1017L7 590l242-241 775 775 775-775z"></path></svg>
    </div>
    <ul tabindex="0" class="dropdown-content menu text-xs text-base-content font-semibold bg-base-300 border border-base-content/20 rounded-sm z-1 w-52 my-2 p-2 shadow-sm">
      <li>
        <label class="label">
          <input type="checkbox" class="checkbox checkbox-xs" id="showTitleYear" v-model="showTitleYear"/>
          Show title and year
        </label>
      </li>
      <li>
        <label class="label">
          <input type="checkbox" class="checkbox checkbox-xs" id="showRating" v-model="showRating"/>
          Show rating
        </label>
      </li>
      <li>
        <label class="label">
          <input type="checkbox" class="checkbox checkbox-xs" id="showDate" v-model="showDate"/>
          Show date
        </label>
      </li>
    </ul>
  </div>

</div>

<!-- Films -->
<div class="flex mx-auto ring-1 bg-base-100 ring-base-content/20 w-auto max-w-xs md:max-w-lg" v-if="currentFilms.length">
  <div ref="captureRef" id="capture" class="bg-linear-to-b from-base-300 via-base-200 to-base-100 aspect-9/16 content-center">
      <!-- Head -->
      <div>
          <div class="flex text-[7px] md:text-xs text-base-content tracking-widest -mb-5 md:-mb-4 px-4">
              {{ profileInfo['Given Name'] || profileInfo['Username'] }}'s Films
          </div>
          <div class="flex justify-between mx-4 py-2 border-b border-base-content/20">
              <h1 class="text-lg md:text-3xl mt-2 text-base-content [:root:has(.theme-controller[value=letterboxd]:checked)_&]:text-white tracking-wide uppercase font-bold">{{ selectedMonth }}</h1>
              <span class="text-[7px] md:text-xs mb-1 text-base-content tracking-widest my-auto">{{ currentFilms.length }} {{ currentFilms.length > 1 ? 'films' : 'film' }}</span>
          </div>
      </div>
      <!-- Grid -->
      <div class="grid gap-0.5 md:gap-1 p-4" :class="dense ? 'grid-cols-6' : 'grid-cols-4'">
          <div v-for="film in currentFilms" :key="film.Name">
              <!-- Poster -->
              <div class="relative aspect-2/3">
                <div v-if="showDate" class="z-1 absolute text-[10px] text-neutral-content text-shadow-xs shadow-black drop-shadow-[0_1.2px_1.2px_rgba(0,0,0,0.8)] font-bold p-1">{{ film['Watched Date'].slice(8, 10) }}</div>
                <div class="skeleton w-full h-full absolute inset-0 rounded-sm"></div>
                <img
                  @load="film.isPosterLoaded = true" @click="openPosterModal(film)" class="relative w-full h-full object-cover border-1 border-base-content/20 hover:ring-1 hover:ring-primary hover:border-primary rounded-sm cursor-pointer transition-opacity duration-300"
                  :class="{ 'opacity-0': !film.isPosterLoaded }"
                  :src="film.posterUrl"
                />
                <!-- Poster not found: badge to prompt manual add -->
                <div
                  v-if="film.posterFound === false"
                  @click="openPosterModal(film)"
                  class="z-40 absolute top-0 right-0 bg-primary text-primary-content text-[8px] md:text-[10px] font-bold leading-none px-1 py-[1px] rounded-bl-sm rounded-tr-sm cursor-pointer"
                  title="Poster not found - click to add manually"
                >+</div>
              </div>
              <!-- Title -->
              <div v-if="showTitleYear" class="mt-[2px] text-[5px] md:text-[8px] text-base-content uppercase font-semibold tracking-wider mb-0.5"><a class="hover:text-primary transition duration-150 ease-in-out" :href="film['Letterboxd URI']" target="_blank">{{ film.Name }}</a> <span class="opacity-60">{{ film.Year }}</span></div>
              <!-- Rating, Like, Rewatch -->
              <div
                v-if="showRating"
                class="stars rating-row text-primary whitespace-nowrap leading-none mb-1 md:mb-2"
                :class="dense ? 'text-[7px] md:text-[10px]' : 'text-[9px] md:text-[14px]'"
              >
                <div class="material-icons inline-block">{{ getStars(film.Rating) }}</div>
                <span v-if="film.isLiked" class="material-icons text-secondary mr-[2px] ml-[2px]">&#xe87d;</span>
                <span v-if="film.Rewatch === 'Yes'" class="material-icons text-base-content/70">&#xe86a;</span>           
              </div>
          </div>
      </div>
  </div>
</div>
<!-- Modal -->
<dialog id="my_modal_2" ref="modalRef" class="modal">
    <div  class="modal-box rounded-sm">
        <form method="dialog">
            <button class="btn btn-sm btn-circle text-base-content btn-ghost absolute right-2 top-2">✕</button>
        </form>
        <h3 class="text-md text-base-content tracking-wider">Change Poster</h3>
        <div class="grid grid-cols-4 gap-1 p-4">
            <div v-for="(poster, index) in paginatedPosters" :key="index" class="relative aspect-2/3">
                <div class="skeleton w-full h-full absolute inset-0 rounded-sm"></div>
                <img @load="poster.isLoaded = true" :class="{ 'opacity-0': !poster.isLoaded }"  class="relative w-full h-full object-cover border-1 border-base-content/20 hover:border-primary hover:ring-1 hover:ring-primary rounded-sm cursor-pointer transition-opacity duration-300" :src="poster.url" @click="selectAlternatePoster(poster.url)"></img>
            </div>
        </div>
        <p class="text-xs text-base-content/50 px-4 pb-2">
          Missing or mismatched posters?
          <span class="text-primary underline cursor-pointer" @click="manualPosterInput?.click()">Add manually.</span>
        </p>
        <div v-if="totalPages > 1" class="modal-action">
          <div class="join">
            <button 
              class="join-item btn rounded-l-sm" 
              @click="currentPage--" 
              :disabled="currentPage === 1"
            >
              «
            </button>
            <button class="join-item btn">
              Page {{ currentPage }} of {{ totalPages }}
            </button>
            <button 
              class="join-item btn rounded-r-sm" 
              @click="currentPage++" 
              :disabled="currentPage === totalPages"
            >
              »
            </button>
          </div>
        </div>
    </div>
    <form method="dialog" class="modal-backdrop">
        <button>close</button>
    </form>
</dialog>
<!-- Hidden file input for manual poster uploads -->
<input
  ref="manualPosterInput"
  type="file"
  accept=".jpg,.jpeg,.png,.webp"
  class="hidden"
  @change="handleManualPoster"
/>
</template>

<script setup>
import { ref, watch, computed } from 'vue';
import Papa from 'papaparse';
import JSZip from 'jszip';
import html2canvas from 'html2canvas-pro';

const showTitleYear = ref(true);
const showRating = ref(true);
const showDate = ref(false);

const themes = {
  Light: ['nord', 'caramellatte', 'autumn', 'valentine', 'lemonade'],
  Dark: ['default', 'letterboxd', 'coffee', 'forest', 'sunset', 'synthwave'],
};

// Pagination
const currentPage = ref(1);
const POSTERS_PER_PAGE = 16;
const totalPages = computed(() =>
  Math.max(1, Math.ceil(postersForModal.value.length / POSTERS_PER_PAGE))
);
const paginatedPosters = computed(() => {
  const start = (currentPage.value - 1) * POSTERS_PER_PAGE;
  return postersForModal.value.slice(start, start + POSTERS_PER_PAGE);
});

// Modal
const modalRef = ref(null); // A ref to hold the <dialog> element
const postersForModal = ref([]);
const filmToUpdate = ref(null); // Keep track of which film is being edited
const manualPosterInput = ref(null); // Hidden file input for manual poster uploads

const openPosterModal = (film) => {
  filmToUpdate.value = film; // Remember which film we're updating

  // If we couldn't find a poster at all, skip the modal and go
  // straight to the file picker so the user can add one manually.
  if (film.posterFound === false) {
    manualPosterInput.value?.click();
    return;
  }

  currentPage.value = 1;
  postersForModal.value = film.alternatePosters || [];
  modalRef.value?.showModal(); // Use the .showModal() method
};
const selectAlternatePoster = (newPosterUrl) => {
  if (filmToUpdate.value) {
    filmToUpdate.value.posterUrl = newPosterUrl; // Update the main poster
    filmToUpdate.value.posterFound = true;
  }
  modalRef.value?.close(); // Close the modal
};

const handleManualPoster = (event) => {
  const file = event.target.files[0];
  if (!file || !filmToUpdate.value) return;
  const objectUrl = URL.createObjectURL(file);
  filmToUpdate.value.posterUrl = objectUrl;
  filmToUpdate.value.isPosterLoaded = false;
  filmToUpdate.value.posterFound = true;
  event.target.value = ''; // reset so same file can be picked again
  modalRef.value?.close();
};

// Upload file
const parsedCsvData = ref([]);
const profileInfo = ref(null);

const readCsv = async (zip, path) =>
  Papa.parse(await zip.file(path).async('string'), { header: true, skipEmptyLines: true }).data;

const handleFileUpload = async (event) => {
  const file = event.target.files[0];
  if (!file) return;

  try {
    const zip = await JSZip.loadAsync(file);
    const [likes, diary, profile] = await Promise.all(
      ['likes/films.csv', 'diary.csv', 'profile.csv'].map((path) => readCsv(zip, path))
    );
    const liked = new Set(likes.map((film) => film.Name));

    profileInfo.value = profile[0];
    parsedCsvData.value = diary.map((film) => ({
      ...film,
      isLiked: liked.has(film.Name),
      posterUrl: null,
      alternatePosters: [],
      isPosterLoaded: false,
      posterFound: null,
      posterRequested: false
    }));
  } catch (error) {
    console.error('Failed to process zip file:', error);
  }
};

const groupedFilms = computed(() => {
  const groups = {};
  for (const film of parsedCsvData.value) {
    if (!film.Date) continue;
    const key = new Date(film['Watched Date']).toLocaleDateString('en-US', {
      month: 'long',
      year: 'numeric',
      timeZone: 'UTC', // date-only strings parse as UTC, so format as UTC too
    });
    (groups[key] ||= []).push(film);
  }
  return groups;
});

// Newest first. Sort on the ISO date ("2026-09"), not by parsing "September 2026",
// which some browsers can't parse (Invalid Date -> the sort silently does nothing).
const months = computed(() => {
  const isoMonth = (label) => groupedFilms.value[label][0]['Watched Date'].slice(0, 7);
  return Object.keys(groupedFilms.value).sort((a, b) => isoMonth(b).localeCompare(isoMonth(a)));
});
const selectedMonth = ref(null);
const currentFilms = computed(() => groupedFilms.value[selectedMonth.value] ?? []);
const dense = computed(() => currentFilms.value.length > 12);

// Default to the most recent month
watch(months, (list) => {
  selectedMonth.value = list[0] ?? null;
});

// Poster
const TMDB_API_KEY = import.meta.env.VITE_TMDB_API_KEY;
const POSTER_BASE = 'https://images.weserv.nl/?url=https://image.tmdb.org/t/p/w500';
const tmdb = (path, params) =>
  fetch(`https://api.themoviedb.org/3/${path}?${new URLSearchParams({ api_key: TMDB_API_KEY, ...params })}`)
    .then((res) => res.json());

const fetchPoster = async (film) => {
  try {
    // Movie first; only fall back to the TV endpoint when there's no movie match
    let type = 'movie';
    let result = (await tmdb('search/movie', { query: film.Name, primary_release_year: film.Year })).results?.[0];
    if (!result) {
      type = 'tv';
      result = (await tmdb('search/tv', { query: film.Name, first_air_date_year: film.Year })).results?.[0];
    }

    if (!result?.poster_path) {
      film.posterUrl = 'https://placehold.co/160x240/374151/FFF?text=Not+Found';
      film.posterFound = false;
      return;
    }

    film.posterUrl = POSTER_BASE + result.poster_path;
    film.posterFound = true;

    const { posters = [] } = await tmdb(`${type}/${result.id}/images`);
    film.alternatePosters = posters.map((p) => ({ url: POSTER_BASE + p.file_path, isLoaded: false }));
  } catch (err) {
    console.error(`Failed to fetch poster for ${film.Name}:`, err);
    film.posterUrl = 'https://placehold.co/160x240/374151/FFF?text=Error';
    film.posterFound = false;
  }
};

// Only fetch posters for the month being viewed, not the whole diary
watch(currentFilms, (films) => {
  films.forEach((film) => {
    if (film.posterRequested) return;
    film.posterRequested = true;
    fetchPoster(film);
  });
});

// Rating: Material Icons codepoints (star / star_half)
const getStars = (rating) =>
  '\uE838'.repeat(Math.floor(rating)) + (rating % 1 >= 0.5 ? '\uE839' : '');

// Export
const captureRef = ref(null)
const handleDownload = async () => {
  const node = captureRef.value
  try {
    const canvas = await html2canvas(node, {
      useCORS: true,
      scale: 2,
    })

    const CROP = 2 // canvas px (scale is 2, so 1 CSS px)
    const trimmed = document.createElement('canvas')
    trimmed.width = canvas.width - CROP * 2
    trimmed.height = canvas.height - CROP * 2
    trimmed.getContext('2d').drawImage(canvas, -CROP, -CROP)

    const link = document.createElement('a')
    link.href = trimmed.toDataURL('image/png')
    link.download = `Wrappedboxd_${selectedMonth.value.replace(' ', '_') || 'download'}.png`
    link.click()
  } catch (err) {
    console.error('Export failed:', err)
  }
}
</script>