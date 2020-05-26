<template>
  <div>
    <hero
      :title="courseHero.title"
      :subtitle="courseHero.subtitle"
      :image="courseHero.image"
      :promoLink="courseHero.product && courseHero.product.productLink"
    />
    <section class="section">
      <div class="container">
        <h1 class="title">Featured Courses</h1>
        <div class="columns is-multiline">
          <div
            v-for="course in courses"
            :key="course._id"
            class="column is-one-quarter"
          >
            <!-- CARD-ITEM -->
            <v-popover offset="16" trigger="hover" placement="right-start">
              <course-card :course="course" />
              <template slot="popover">
                <course-card-tooltip
                  :title="course.title"
                  :subtitle="course.category.name"
                  :description="course.subtitle"
                  :wsl="course.wsl"
                />
              </template>
            </v-popover>
            <!-- CARD-ITEM-END -->
          </div>
        </div>
      </div>
    </section>
    <section class="section">
      <div class="container">
        <h1 class="title">Featured Articles</h1>
        <div class="columns is-multiline">
          <div
            v-for="blog in featuredBlogs"
            :key="blog._id"
            class="column is-one-quarter"
          >
            <!-- CARD-ITEM -->
            <blog-card :key="blog._id" :blog="blog" />
            <!-- CARD-ITEM-END -->
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
import CourseCard from "~/components/CourseCard";
import CourseCardTooltip from "~/components/CourseCardTooltip";
import BlogCard from "~/components/BlogCard";
import Hero from "~/components/shared/Hero";
import { mapState } from "vuex";
export default {
  head: {
    title: "Online Courses and Blogs | YUSUKE KAGOSE",
  },
  components: {
    CourseCard,
    BlogCard,
    Hero,
    CourseCardTooltip,
  },
  computed: {
    ...mapState({
      courses: (state) => state.course.items,
      featuredBlogs: (state) => state.blog.items.featured,
      courseHero: (state) => state.hero.item,
    }),
  },
  async fetch({ store }) {
    await store.dispatch("course/fetchCourses");
    await store.dispatch("blog/fetchFeaturedBlogs", {
      "filter[featured]": true,
    });
  },
};
</script>

<style scoped lang="scss">
// card item
.card-image:hover {
  cursor: pointer;
  opacity: 0.9;
}
.price-box {
  text-align: right;
  .price {
    color: gray;
    font-size: 16px;
    text-decoration: line-through;
  }
  .disc-price {
    font-size: 21px;
    font-weight: bold;
  }
}
// card item end
// hero

// hero
// Home page
.links {
  padding-top: 15px;
}
</style>
