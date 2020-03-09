<template>
  <div class = "Titre">
  <h1> Bienvenue sur Exoplanètes</h1>
  <p>Cette application web permet de visualiser pour plusieurs exoplanètes quelques caractéristiques ainsi que l'évolution de leur révolution autour de leur étoile</p>
  <p>Cliquez sur "Visualisation" puis sélectionnez une exoplanète (certaines exoplanètes n'ont pas de données fournies pour leur rayon/masse/température)</p>
  </div>
</template>

<script>
export default {
  head() {
    return {
      title: "Bienvenue sur Exoplanetes",
      meta: [
        {
          hid: "description",
          name: "description",
          content: "Visualisation de la révolution d'exoplanètes autour de leur étoile"
        }
      ]
    };
  }
};
</script>
