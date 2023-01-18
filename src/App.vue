<template>
  <div class="app-wrapper">
    <div class="header">
      <div class="header__row">
        <div class="header__name">Имя</div>
        <div class="header__lenght">Размер</div>
        <div class="header__type">Тип</div>
      </div>
      <div class="header__row">
        <input
          class="search-form header__search-form"
          type="text"
          placeholder="Введите имя файла/папки для поиска"
          v-model="searchText"
        />
      </div>
    </div>

    <FolderView
      :name="mainName"
      :folders="activeData.folders"
      :files="activeData.files"
      :isOpen="true"
    />
  </div>
</template>

<script>
import FolderView from "./components/FolderView.vue";
import data from "./data/list";

export default {
  name: "App",
  components: {
    FolderView,
  },
  data: function() {
    return {
      folders: [],
      files: [],
      searchText: "",
      findedFiles: {
        files: new Set(),
        folders: new Set(),
      },
    };
  },
  computed: {
    activeData: function() {
      if (this.searchText.length > 0) {
        return this.findedFiles;
      } else {
        return {
          folders: this.folders,
          files: this.files,
        };
      }
    },
    mainName: function() {
      return this.searchText.length > 0 ? "Найденные файлы и папки" : "root";
    },
  },
  created() {
    //вытаскиваем информацию из адрессной строки
    const windowData = Object.fromEntries(
      new URL(window.location).searchParams.entries()
    );
    //заготовочка на будущее
    const VALID_KEYS = ["searchText"];
    //устанавливаем стэйт
    this.files = data.files;
    this.folders = data.folders;

    VALID_KEYS.forEach((key) => {
      if (windowData[key]) {
        this[key] = windowData[key];
        //если в строке нет параметра он берется из localStorage
      } else {
        let item = JSON.parse(localStorage.getItem(key));
        if (item) {
          this[key] = item;
        }
      }
    });
  },
  watch: {
    searchText: function() {
      //сохраняем информацию о поиске
      history.pushState(null, " ", `?searchText=${this.searchText}`);
      localStorage.setItem("searchText", JSON.stringify(this.searchText));
      //Поиск файлов
      this.findedFiles.folders = new Set();
      this.findedFiles.files = new Set();
      this.findItems(this.folders, this.files, this.searchText);
    },
  },
  methods: {
    //Реализация поиска файлов и папок в глубину
    findItems(folders, files, pattern) {
      if (files == null) return;
      if (folders == null) return;
      files.forEach((file) => {
        if (file.name.includes(pattern)) {
          this.findedFiles.files.add(file);
        }
      });
      folders.forEach((folder) => {
        if (folder.name.includes(pattern)) {
          this.findedFiles.folders.add(folder);
        }
        this.findItems(folder.folders, folder.files, pattern);
      });
      return;
    },
  },
};
</script>

<style lang="scss">
@import "@/styles/style.scss";
</style>
