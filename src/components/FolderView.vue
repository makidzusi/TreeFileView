<template>
  <ul class="folders-list">
    <div class="folders-list__wrapper" @click="isOpen = !isOpen" tabindex="0" @keydown.space="isOpen = !isOpen">
      <div class="folders-list__folder">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          width="16"
          height="16"
          fill="currentColor"
          class="bi bi-folder-fill"
          viewBox="0 0 16 16"
        >
          <path
            d="M9.828 3h3.982a2 2 0 0 1 1.992 2.181l-.637 7A2 2 0 0 1 13.174 14H2.825a2 2 0 0 1-1.991-1.819l-.637-7a1.99 1.99 0 0 1 .342-1.31L.5 3a2 2 0 0 1 2-2h3.672a2 2 0 0 1 1.414.586l.828.828A2 2 0 0 0 9.828 3zm-8.322.12C1.72 3.042 1.95 3 2.19 3h5.396l-.707-.707A1 1 0 0 0 6.172 2H2.5a1 1 0 0 0-1 .981l.006.139z"
          />
        </svg>
        <span class="folders-list__folder-name" style="cursor: pointer">
          {{ name }}
        </span>
        <svg
          xmlns="http://www.w3.org/2000/svg"
          width="12"
          height="12"
          fill="currentColor"
          class="bi bi-arrow-90deg-down"
          viewBox="0 0 16 16"
          v-if="isOpen"
          style="transform: scale(-1, 1)"
        >
          <path
            fill-rule="evenodd"
            d="M4.854 14.854a.5.5 0 0 1-.708 0l-4-4a.5.5 0 0 1 .708-.708L4 13.293V3.5A2.5 2.5 0 0 1 6.5 1h8a.5.5 0 0 1 0 1h-8A1.5 1.5 0 0 0 5 3.5v9.793l3.146-3.147a.5.5 0 0 1 .708.708l-4 4z"
          />
        </svg>
        <svg
          xmlns="http://www.w3.org/2000/svg"
          width="16"
          height="16"
          fill="currentColor"
          class="bi bi-arrow-right"
          viewBox="0 0 16 16"
          v-if="!isOpen"
        >
          <path
            fill-rule="evenodd"
            d="M1 8a.5.5 0 0 1 .5-.5h11.793l-3.147-3.146a.5.5 0 0 1 .708-.708l4 4a.5.5 0 0 1 0 .708l-4 4a.5.5 0 0 1-.708-.708L13.293 8.5H1.5A.5.5 0 0 1 1 8z"
          />
        </svg>
      </div>
      <div v-if="folders.length >= 0" class="folders-list__length">
        {{ folders.length }} subfolder(s)
      </div>
      <div v-else class="folders-list__length"></div>
      <div class="folders-list__type">directory</div>
    </div>

    <template v-if="isOpen">
      <li
        class="folders-list__item"
        :key="folder.name"
        v-for="folder in folders"
      >
        <FolderView
          :name="folder.name"
          :folders="folder.folders"
          :files="folder.files"
          :isOpen="false"
        />
      </li>
      <li :key="file.name" v-for="file in files">
        <FileView
          :name="file.name"
          :type="file.type"
          :length="file.length"
        /></li
    ></template>
  </ul>
</template>

<script>
import FileView from "./FileView";
export default {
  name: "FolderView",
  components: { FileView },
  props: {
    name: String,
    folders: [Set, Array],
    files: [Set, Array],
    depth: Number,
    isOpen: Boolean,
  },
};
</script>
