<script>
import titleSecundary from "@/components/TitleSecundary.vue";
import cardTeam from "@/components/CardTeam.vue";
import client from "@/plugins/contentful.js";

export default {
  data() {
    return {
      metaContent: {},
      teamCont: {},
    };
  },
  components: {
    titleSecundary,
    cardTeam,
  },
  head() {
    if (
      this.metaContent[0] &&
      this.metaContent[0].fields.title &&
      this.metaContent[0].fields.description &&
      this.metaContent[0].fields.image.fields.file.url
    ) {
      return {
        title: this.metaContent[0].fields.title,
        meta: [
          {
            name: "description",
            content: this.metaContent[0].fields.description,
          },
          {
            hid: "og:image",
            content: this.metaContent[0].fields.image.fields.file.url,
          },
          {
            name: "keywords",
            content: this.metaContent[0].fields.description,
          },
          { hid: "og:title", content: this.metaContent[0].fields.title },
          {
            hid: "og:image",
            content: this.metaContent[0].fields.image.fields.file.url,
          },
          {
            hid: "og:description",
            content: this.metaContent[0].fields.description,
          },
          {
            name: "twitter:title",
            content: this.metaContent[0].fields.title,
          },
          {
            name: "twitter:description",
            content: this.metaContent[0].fields.description,
          },
          {
            name: "twitter:image",
            content: this.metaContent[0].fields.image.fields.file.url,
          },
        ],
      };
    }
  },
  async asyncData() {
    const team = await client.getEntries({
      content_type: "teamNwc",
      order: "fields.order",
    });

    const metaPage = await client.getEntries({
      content_type: "metaPage",
      "fields.slugPage": "team",
    });

    return {
      teamCont: team.items,
      metaContent: metaPage.items,
    };
  },
 
};
</script>
<template>
  <main>
    <section class="team-section">
      <div class="p-team m-0">
        <div class="row m-0 p-0">
          <div
            class="col-md-12 p-0 m-0 align-items-center d-flex flex-column text-center"
          >
            <div class="d-flex mt-0 mr-0 ml-0 p-0 w-100 container-secundary justify-content-center">
              <h2 class="title-secundary">Who we are</h2>
            </div>
            <p class="text-medium text-team">
              We&#8217;re a team of investors, innovators, and industry experts
              passionate about developing the next-generation of life-changing
              mobility technologies.
            </p>
          </div>
        </div>
        <div class="container-team p-0">
          <card-team
            v-for="(team, index) in teamCont"
            :key="index"
            :teamName="team.fields.title"
            :teamJob="team.fields.designation"
            :teamImage="team.fields.profilePic.fields.file.url"
            :teamEmail="team.fields.emailId"
            :teamSlug="team.fields.slug"
          >
          </card-team>
        </div>
      </div>
    </section>
  </main>
</template>
<style scoped>

.container-secundary{
  margin-bottom: 1.5rem;
}
.title-secundary {
  font-weight: 500;
  font-style: normal;
  letter-spacing: -2px;
  color: var(--color--secondary);
}
.p-team {
  padding: 4%;
}
.text-team {
  color: var(--color--secondary);
  width: 80%;
}
.container-team {
  margin-top: 4%;
}
p.text-team {
  max-width: 1022px;
}
.team-section {
  min-height: 93vh;
}
@media (min-width: 1441px) {
  .title-secundary {
    font-size: 45px;
    line-height: 51.75px;
  }
}
@media (max-width: 1440px) {
  .title-secundary{
    font-size: 40px;
    line-height: 48px;
  }
}
@media (max-width: 767px) {
  h2.title-secundary {
    padding: 0;
    width: 100%;
    font-size: 35px;
    line-height: 39px;
  }
}
@media (max-width: 1024px) {
  .team-section {
    min-height: 100%;
  }
}
@media (max-width: 767px) {
  .p-team {
    padding: 60px 40px 40px;
  }
}
</style>
