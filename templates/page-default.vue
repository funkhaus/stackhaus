<template>
    <section class="page-home">
        <wp-image
            class="image"
            :image="parsedPage.featuredImage"
        />

        <wp-gutenberg
            class="content"
            :blocks="parsedPage.blocks"
        />
    </section>
</template>

<script>
// Helpers
import _get from "lodash/get"

// Queries
import HOME from "~/gql/queries/Home"

export default {
    async asyncData({ $graphql, route }) {
        const data = await $graphql.default.request(HOME, {
            uri: route.path,
        })
        return {
            page: _get(data, "nodeByUri", {}),
        }
    },
    computed: {
        parsedPage() {
            // Shape data from WP-GQL to work with template
            return {
                ...this.page,
                featuredImage: _get(this, "page.featuredImage.node", {}),
            }
        },
    },
}
</script>

<style lang="scss" scoped>
.page-home {
    color: var(--color-black);
    margin: 0 auto;
    min-height: var(--unit-100vh);
    text-align: center;

    .image {
        max-width: 50%;
        margin: 0 auto;
    }

    // Hover states
    // @media #{$has-hover} {
    // Hover styles would go here
    // }

    // Breakpoints
    // @media #{$lt-tablet} {
    // }
}
</style>
