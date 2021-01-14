<template>
  <div id="linkverwaltung-app" class="container">
    <header>
      <h1>Franz Kohnles Linkverwaltung 7</h1>
    </header>
    <main>
      <!-- Eingabeformular -->
      <NeuerLink @bestaetigen="hinzufuegen" />

      <!-- Ausgabeliste -->
      <ul class="list-group">
        <ListItem
          v-for="link in links"
          :key="link.id"
          :link="link"
          @upvote="upvote(link.id)"
          @delete-link="deleteLink(link.id)"
        />
      </ul>
    </main>
    <LinkverwaltungFooter />
  </div>
</template>

<script>
import LinkverwaltungFooter from "./components/LinkverwaltungFooter.vue";
import NeuerLink from "./components/NeuerLink.vue";
import ListItem from "./components/ListItem.vue";

export default {
  name: "App",
  components: {
    LinkverwaltungFooter,
    NeuerLink,
    ListItem,
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
    hinzufuegen(newLink) {
      newLink.id = this.nextId;
      this.nextId = this.nextId + 1;
      this.links.push(newLink);
      this.speichern();
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
