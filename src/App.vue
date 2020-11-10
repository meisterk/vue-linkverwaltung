<template>
  <HelloWorld msg="Das ist noch die Component" />
  <div id="linkverwaltung-app" class="container">
    <header>
      <h1>Linkverwaltung</h1>
    </header>
    <main>
      <div class="border mb-2 bg-light p-2">
        <legend>Neuer Link</legend>
        <div class="form-group">
          <label for="inputName">Name</label>
          <input
            v-model="newName"
            class="form-control"
            id="inputName"
            type="text"
            placeholder="Name"
          />
        </div>

        <div class="form-group">
          <label for="inputUrl">Url</label>
          <input
            v-model="newUrl"
            class="form-control"
            id="inputUrl"
            type="text"
            placeholder="Url"
          />
        </div>

        <button
          @click="hinzufuegen"
          class="btn btn-primary mb-1"
          aria-label="Hinzufügen"
        >
          Hinzufügen
        </button>
      </div>
      <ul class="list-group">
        <li
          class="list-group-item d-flex justify-content-between"
          v-for="link in links"
          :key="link.id"
        >
          <a :href="link.url">{{ link.linktext }}</a>
          <div>
            <span class="badge badge-primary">{{ link.votes }}</span>
            <button
              class="btn btn-primary btn-sm ml-1"
              @click="upvote(link.id)"
              aria-label="Hochvoten"
            >
              <i class="lni lni-thumbs-up"></i>
            </button>
            <button
              class="btn btn-danger btn-sm ml-1"
              @click="deleteLink(link.id)"
              aria-label="Löschen"
            >
              <i class="lni lni-trash"></i>
            </button>
          </div>
        </li>
      </ul>
    </main>
    <div class="footer mt-2">
      <footer class="border alert alert-info">
        Diese Links werden wurden von Franz Kohnle gesammelt.
      </footer>
    </div>
  </div>
</template>

<script>
import HelloWorld from "./components/HelloWorld.vue";

export default {
  name: "App",
  components: {
    HelloWorld,
  },
  data() {
    return {
      links: [
        {
          id: 0,
          linktext: "kohnlehome.de",
          url: "http://kohnlehome.de",
          votes: 1,
        },
        {
          id: 1,
          linktext: "Offizielle Website der GBS",
          url: "https://gbsschulen.de",
          votes: 3,
        },
      ],
      nextId: 4,
      newName: "",
      newUrl: "",
    };
  },
  methods: {
    upvote(id) {
      console.log("+");
      //this.links[id].votes++; Funktioniert nicht, wenn umsortiert wurde
      const clickedLink = this.links.find((link) => link.id === id);
      clickedLink.votes++;
      this.sortieren();
      this.speichern();
    },
    deleteLink(id) {
      // index des zu löschenden Links ermitteln
      let index = this.links.findIndex((link) => link.id === id);
      // Element an der Stelle index aus Array entfernen
      this.links.splice(index, 1);
      this.speichern();
    },
    sortieren() {
      this.links.sort(function (link1, link2) {
        return link2.votes - link1.votes;
      });
    },
    laden() {
      if (localStorage.getItem("links")) {
        const linksString = localStorage.getItem("links");
        this.links = JSON.parse(linksString);
      } else {
        this.links = [
          {
            id: 0,
            linktext: "kohnlehome.de",
            url: "http://kohnlehome.de",
            votes: 1,
          },
          {
            id: 1,
            linktext: "Offizielle Website der GBS",
            url: "https://gbsschulen.de",
            votes: 3,
          },
        ];
      }
    },
    speichern() {
      const linksString = JSON.stringify(this.links);
      localStorage.setItem("links", linksString);
    },
    hinzufuegen() {
      if (this.newName != "" && this.newUrl != "") {
        const newLink = {
          id: this.nextId,
          linktext: this.newName,
          url: this.newUrl,
          votes: 0,
        };
        this.nextId = this.nextId + 1;
        this.links.push(newLink);
        this.speichern();
      }
    },
  },
  mounted() {
    this.laden();
    this.sortieren();
  },
};
</script>

<style>
</style>
