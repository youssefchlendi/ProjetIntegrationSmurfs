<template>
  <div class="container bg-white p-4 card">
    <h1>Mes demande d'adhesion</h1>
    <b-alert class="mt-4" :show="alert.dismissCountDown" dismissible :variant="alert.variant"
      @dismissed="alert.dismissCountDown = 0">
      <p>{{ alert.msg }}</p>
    </b-alert>
    <main>
      <data-table v-bind="parameters" @actionTriggered="handleAction" />
    </main>
  </div>
</template>

<script>
export default {
  data() {
    return {
      demandes: [],
      alert: {
        dismissCountDown: 0,
        variant: "",
        msg: "",
      },
    }
  },
  created() {
    this.getDemandes();
    
    if (this.$route.params.nomClub != null) {
      this.alert.variant = "success";
      this.alert.msg = "Demande d'adhesion envoyée avec succès";
      this.alert.dismissCountDown = 5;
    }
  },
  methods: {
    getDemandes() {
      this.$http.get("http://localhost:8000/api/dac/a").then(response => {
        this.demandes = response.data.data
        this.demandes.forEach(demande => {
          demande.nomClub = demande.club.demande_creation_club.nomClub;
          demande.status = demande.status == 0 ? "En attente" : demande.status == 1 ? "Acceptée" : "Refusée";
        })
      })
    },
    handleAction(action, data) {
      if (action == 'delete') {
        if (confirm("Suprimer demande ?")) {
          this.$http.delete("http://localhost:8000/api/dac/" + data.id).then(() => {
            this.alert.variant = "danger";
            this.alert.msg = "Demande d'adhesion supprimée avec succès";
            this.alert.dismissCountDown = 5;
            this.getDemandes()
          })
        }
      }
    }
  },
  computed: {
    parameters() {
      return {
        data: this.demandes,
        text: {
          perPageText: 'Afficher :entries lignes',
          infoText: 'Affichage de :first a :last de :total lignes',
          infoTextFiltered: 'Affichage de :first a :last de :total lignes(filtrée de :total)',
          nextButtonText: 'Suiva',
          previousButtonText: 'Prec',
          paginationSearchText: 'Aller vers page',
          searchText: 'recherche',
          downloadButtonText: 'TELECHARGER',
          emptyTableText: 'Aucun demande trouvée'
        },
        actionMode: "single",
        actions: ['delete']
        ,
        columns: [
          {
            key: "nomClub",
            title: "Nom Club",
          },
          {
            key: "status",
            title: "Status",

          },
          {
            key: "message",
            title: "Message"
          }
        ]
      };
    }
  },
}
</script>

<style>
</style>