<template>
  <div>
    <div
      class="gallery"
      v-for="(category, index) in categories"
      :key="'category' + index"
    >
      <h2 class="gallery__title title">{{ category.title }}</h2>
      <ul class="gallery__list">
        <li
          class="gallery__item"
          v-for="experience in category.experiences"
          :key="'experience' + experience.id"
          @click="
            $router.push({
              name: 'Experience',
              params: { id: experience.id }
            })
          "
        >
          <ImageRatio
            class="gallery__item__image"
            :url="require('@/assets/experiences/' + experience.image)"
            ratio="145%"
          />
          <router-link
            @click.native.stop
            class="gallery__item__link stopPropagation"
            :to="{ name: 'Experience', params: { id: experience.id } }"
          >
            <GalleryTitleClamp
              :price="getPrice(experience.price)"
              :title="experience.title"
              :lineLimit="2"
            />
          </router-link>
        </li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
.gallery:not(:last-child) {
  margin-bottom: var(--bottom-separator-height);
}

.gallery__title {
  font-size: var(--f32);
  margin-bottom: 2rem;
}

.gallery__list {
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  column-gap: 1rem;
  row-gap: 1rem;
  list-style-type: none;
  margin: 0;
  padding: 0;
}

@media (max-width: 1200px) {
  .gallery__list {
    grid-template-columns: repeat(2, 1fr);
  }
}

.gallery__item {
  display: flex;
  flex-direction: column;
  width: 100%;
  cursor: pointer;
}

.gallery__item__image {
  margin-bottom: 0.5rem;
}

.gallery__item__link {
  text-decoration: none;
  color: #484848;
  font-family: "SF UI Text Light", Avenir, Helvetica, Arial, sans-serif;
  font-weight: 300;
  font-size: var(--f15);
  line-height: 1.2;
}

.gallery__item:hover > .gallery__item__link {
  text-decoration: underline;
}
</style>

<script>
import data from "@/assets/data.json";
import ImageRatio from "@/components/ImageRatio.vue";
import GalleryTitleClamp from "@/components/GalleryTitleClamp.vue";
import moneyParser from "@/mixins/moneyParser.js";

export default {
  components: {
    ImageRatio,
    GalleryTitleClamp
  },
  mixins: [moneyParser],
  data() {
    return {
      allCategories: [
        {
          title: "Just Booked",
          experiences: data["experiences"].filter(
            xp => xp.category === "booked"
          )
        },
        {
          title: "Featured experiences",
          experiences: data["experiences"].filter(
            xp => xp.category === "featured"
          )
        }
      ]
    };
  },
  computed: {
    categories: function() {
      return this.allCategories.filter(
        category => category.experiences.length > 0
      );
    }
  },
  methods: {
    getPrice: function(value) {
      return `$${this.americanNumberFormat(value)}`;
    }
  }
};
</script>
