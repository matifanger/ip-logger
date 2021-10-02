<template>
    <div>
        <v-data-table
        v-if="QueryResponse == true"
          :items-per-page="itemsPerPage"
          :hide-default-footer="hideFooter"
          :headers="headers"
          :items="desserts"
          class="elevation-1 overflow-y-auto ma-10"
          id="table"
        >


          <template v-slot:[`item.IP`]="{ item }">
            {{item.data.ip.ip}}
          </template>

          <template v-slot:[`item.DATE`]="{ item }">
            {{item.data.system.date}}
          </template>

        <template v-slot:[`item.LAT`]="{ item }">
            {{item.data.ip.latitude}}
          </template>

        <template v-slot:[`item.LON`]="{ item }">
            {{item.data.ip.longitude}}
          </template>

        <template v-slot:[`item.IP_REGION`]="{ item }">
            {{item.data.ip.timezone}}
          </template>

        <template v-slot:[`item.SYS_REGION`]="{ item }">
            {{item.data.system.timeZone}}
          </template>

        <template v-slot:[`item.NAV_LANG`]="{ item }">
            {{item.data.navigator.language}}
          </template>

        <template v-slot:[`item.SYS_LANG`]="{ item }">
            {{item.data.system.language}}
          </template>

        <template v-slot:[`item.NAV_PLATFORM`]="{ item }">
            {{item.data.navigator.platform}}
          </template>

        <template v-slot:[`item.NAV_APP`]="{ item }">
            {{item.data.navigator.appCodeName}}
          </template>

        <template v-slot:[`item.DELETE`]="{ item }">
            <v-icon red v-on:click="deleteItem(item)">mdi-delete</v-icon>
        </template>

        </v-data-table>

    </div>
</template>

<script>
    export default {
        data() {
            return {
                season: 0,
                itemsPerPage: 20,
                hideFooter: true,
                ready: false,
                api_key: "68f1d317-cfbf-4658-b213-012a89242ca2",
                QueryResponse: null,
                headers: [
                    { text: "DATE", value: "DATE", align: "center" },
                    { text: "IP", value: "IP", align: "center" },
                    { text: "LAT", value: "LAT", align: "center" },
                    { text: "LON", value: "LON", align: "center" },
                    { text: "IP REGION", value: "IP_REGION", align: "center" },
                    { text: "SYS REGION", value: "SYS_REGION", align: "center" },
                    { text: "NAV LANG", value: "NAV_LANG", align: "center" },
                    { text: "SYS LANG", value: "SYS_LANG", align: "center" },
                    { text: "PLATFORM", value: "NAV_PLATFORM", align: "center" },
                    { text: "APP", value: "NAV_APP", align: "center" },
                    { text: "DELETE", value: "DELETE", align: "center" },
                ],
                desserts: [],
            }
        },
        methods: {
            deleteItem(item) {

                this.$fire.firestore.collection("logs").doc(item.id).delete().then(() => {
                    console.log("Document successfully deleted!");
                    this.getDB()
                }).catch((error) => {
                    console.error("Error removing document: ", error);
                });

                console.log('deleted', item)
            },
            getDB() {
                this.desserts = [],
                        this.$fire.firestore
        .collection("logs")
        .get()
        .then((querySnapshot) => {
          if (querySnapshot.empty) {
            return (this.QueryResponse = false);
          }
          querySnapshot.forEach((doc) => {
            // doc.data() is never undefined for query doc snapshots
            var data = doc.data()
            data.id = doc.id
            console.log(doc.id, " => ", doc.data());

            this.desserts.push(data);
          });
          this.QueryResponse = true;
        })
        .catch(function (error) {
          console.log("Error getting documents: ", error);
          return (this.QueryResponse = false);
        })
            }
        },
        mounted() {
            this.getDB()
        }
    }
</script>

<style lang="scss" scoped>

</style>