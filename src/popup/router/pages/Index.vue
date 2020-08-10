<script>
export default {
  data() {
    return {
      hover: 999,
      rootFolders: [],
      displayFolder: [],
      displayList: [],
      routing: [],
      inHomePage: true,
      topTitle: 'Bookmarks Bar',
      folderModal: false,
      linkModal: false,
      inputFolderName: '',
      inputLinkTitle: '',
      inputLinkUrl: ''
    };
  },
  created () {
    this.getData();
  },
  methods: {
    // first data
    getData () {
      var gettingTree = browser.bookmarks.getTree();
      this.routing.push("1");
      gettingTree.then(this.logTree, this.onRejected);
    },
    logTree(bookmarkItems) {
      this.rootFolders = bookmarkItems[0].children;
      // when initialize , display first tab of top menu
      this.displayFolder = this.rootFolders[0].children.filter(x => x.url === undefined);
      this.displayList = this.rootFolders[0].children.filter(x => x.url !== undefined);
    },
    clickTopBar(id, title) {
      this.topTitle = title;
      this.routing = [];
      this.routing.push(id);
      this.getDataById(id);
    },
    openFolder(id) {
      this.routing.push(id);
      this.getDataById(id);
      this.inHomePage = false;
    },
    goBack() {
      if (this.routing.length > 1) {
        let selfId = this.routing.pop();
        let parentId = this.routing[this.routing.length - 1];
        if (this.routing.length === 1){
          this.inHomePage = true;
        }
        this.getDataById(parentId);
      }
    },
    getDataById(id) {
      var gettingBookmarks = browser.bookmarks.getChildren(id);
      this.folderId = id;
      gettingBookmarks.then(this.onFulfilled, this.onRejected);
    },
    onFulfilled(bookmarks) {
      this.displayFolder = bookmarks.filter(x => x.url === undefined);
      this.displayList = bookmarks.filter(x => x.url !== undefined);
    },
    onRejected(error) {
      console.log(`An error: ${error}`);
    },
    // getting browser bookmarks data
    openLink(url) {
      window.open(url);
    },
    openFolderCreation () {
      this.folderModal = true;
    },
    openLinkCreation () {
      this.linkModal = true;
    },
    createFolder (name) {
      browser.bookmarks.create({ parentId: this.routing[this.routing.length - 1], title: name });
      this.folderModal = false;
      this.getDataById(this.routing[this.routing.length - 1]);
    },
    createLink(name, url) {
      var createBookmark = browser.bookmarks.create({
        title: this.inputLinkTitle,
        url: this.inputLinkUrl,
        parentId: this.routing[this.routing.length - 1]
      });
      createBookmark.then(this.getDataById(this.routing[this.routing.length - 1]));
      this.linkModal = false;
    },
    onCreated(node) {
      console.log(node);
    },
    deleteLink (id, parentId) {
      var removingBookmark = browser.bookmarks.remove(id);
      removingBookmark.then(this.getDataById(parentId));
    },
    deleteFolder (id, parentId) {
      event.stopPropagation(); 
      var removingBookmark = browser.bookmarks.remove(id);
      removingBookmark.then(this.getDataById(parentId));
    },
    cancel() {
      this.folderModal = false;
    },
    cancelLink() {
      this.linkModal = false;
    },
    mouseover(index) {
      console.log(index);
      this.hover = index;
    }
  },
};
</script>

<template>
  <div class="main">
    <div class="header">
      <div class="header-widget" 
        @click="clickTopBar(array.id, array.title)"
        v-for="array in rootFolders" 
        :key="array.title"
        :style="{ borderBottom: array.title === topTitle ? '4px solid #00b7c2' : 'none'}"
      >
        {{ array.title }}
      </div>
    </div>

    <div class="body">
      <div class="body-left">
        <div class="body-left-folders">
          <div class="subMenu" 
            @click="openFolder(folder.id)" 
            v-for="(folder, index) in displayFolder" 
            :key="folder.title"
            @mouseover="mouseover(index)"
            @mouseleave="hover = 999"
          >
            <i class="icon" :class="{'icon-folder-open-o': hover === index, 'icon-folder-o': hover !== index}"/>
            <div class="subMenu-title">{{ folder.title }}</div>
            <i class="icon" :class="{'icon-bin': hover === index}" @click="deleteFolder(folder.id, folder.parentId)"/>
          </div>
        </div>
        <div class="body-list">
          <div class="subList" v-for="list in displayList" :key="list.title">
            <div class="subList-text" @click="openLink(list.url)">{{ list.title }}</div>
            <i class="icon icon-bin" @click="deleteLink(list.id, list.parentId)"/>
          </div>
        </div>
      </div>
      <div class="body-right">
        <div class="body-right-menu">
          <i class="icon icon-home" v-if="inHomePage" />
          <i class="icon icon-arrow-left2" v-if="!inHomePage" @click="goBack()"/>
        </div>
        <div class="body-right-menu">
          <i class="icon icon-folder-add" @click="openFolderCreation()"/>
        </div>
        <div class="body-right-menu">
          <i class="icon icon-note-add" @click="openLinkCreation()"/>
        </div>
      </div>
    </div>

    <div class="modal-mask" v-if="folderModal">
      <div class="modal-wrapper">
        <div class="modal-container">
          <input v-model="inputFolderName" placeholder="Folder Name">
          <button @click="createFolder(inputFolderName)">create</button>
          <button @click="cancel()">Cancel</button>
        </div>
      </div>
    </div>

    <div class="modal-mask" v-if="linkModal">
      <div class="modal-wrapper">
        <div class="modal-container">
          <div>
            <input v-model="inputLinkTitle" placeholder="TITLE NAME">
            <input v-model="inputLinkUrl" placeholder="URL">
          </div>
          <button @click="createLink(inputLinkTitle, inputLinkUrl)">create</button>
          <button @click="cancelLink()">cancel</button>
        </div>
      </div>
    </div>

  </div>
</template>



<style lang="scss" scoped>
p {
  font-size: 20px;
}

.main {
  width: 320px;
  height: 550px;
  margin: 0;
  background-color: rgb(28,30,32);
  .header {
    height: 50px;
    width: 100%;
    background-color: #242628;
    color: black;
    display: flex;
    flex-direction: row;
    &-widget {
      width: 33%;
      height: 100%;
      display: flex;
      justify-content: center;
      align-items: center;
      box-sizing: border-box;
      cursor: pointer;
      color: white;
      padding: 0 10px;
      text-align: center;
    }
  }
}
.body {
  height: 500px;
  overflow: auto;
  font-size: 14px;
  display: flex;
  flex-direction: column;
  &-left {
    width: 320px;
    height: 100%;
    overflow: auto;
    &-folders {
      display: flex;
      flex-wrap: wrap;
      margin-right: 10px;
      margin-left: 10px;
      .subMenu {
        // background-color: #f3f6f9;
        background-color: rgb(36,38,40);
        color: white;
        display: flex;
        height: 50px;
        width: 44%;
        margin: 10px 7px;
        border-radius: 5px;
        cursor: pointer;
        position: relative;
        box-sizing: border-box;
        border: 1px solid #00b7c2;
        &-title {
          position: absolute;
          display: flex;
          left: 35px;
          height: 50px;
          top: 0;
          align-items: center;
          width: 80px;
          text-align: center;
        }
        .icon-folder-o, .icon-folder-open-o{
          font-size: 20px;
          position: absolute;
          left: 10px;
          top: 15px;
        }
        .icon-bin {
          position: absolute;
          right: 5px;
          top: 15px;
        }
      }
      .subMenu:hover {
        background-color: #0d4d90;
        color: white;
      }
    }
  }
  &-left::-webkit-scrollbar {
    display: none;
  }
  &-right {
    width: 320px;
    background-color: #242628;
    border-top: 1px solid #00b7c2;
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: space-between;
    padding: 0 10px;
    box-sizing: border-box;
    &-menu {
      color: white;
      border-radius: 50%;
      width: 35px;
      height: 35px;
      align-items: center;
      justify-content: center;
      display: flex;
      margin: 10px 0;
      cursor: pointer;
      .icon {
        font-size: 25px;
      }
    }
    &-menu:hover {
      background-color: white;
      color: #0d4d90;
    }
  }
  
  &-list {
    margin: 0 10px;
    .subList {
      padding: 8px 0;
      line-height: 20px;
      display: flex;
      flex-direction: row;
      justify-content: space-between;
      align-items: center;
      color: white;
      &-text {
        cursor: pointer;
        margin-right: 10px;
      }
      .icon-bin {
        color: #00b7c2;
      }
    }
  }
}
.body::-webkit-scrollbar {
  display: none;
}

.modal-mask {
  position: fixed;
  z-index: 9998;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: table;
  transition: opacity 0.3s ease;
}
.modal-wrapper {
  display: table-cell;
  vertical-align: middle;
}
.modal-container {
  width: 300px;
  margin: 0px auto;
  padding: 20px 30px;
  background-color: #fff;
  border-radius: 2px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.33);
  transition: all 0.3s ease;
}
.modal-body {
  margin: 20px 0;
}

</style>
