<script setup lang="ts">
import { computed, reactive, ref } from "vue";
import ProductItem from "./components/ProductItem/ProductItem.vue";
import ActionAndFilters from "./components/ActionAndFilters.vue";

interface Product {
    id: number;
    title: string;
    price: number;
    inStock: boolean;
    imageUrl: string;
}

const products = ref<Product[]>([
    {
        id: 1,
        title: "纯棉 T 恤",
        price: 66,
        inStock: true,
        imageUrl: "/images/t-shirt1.jpg",
    },
    {
        id: 2,
        title: "夹克外套",
        price: 128,
        inStock: true,
        imageUrl: "/images/jacket1.jpg",
    },
    {
        id: 3,
        title: "水洗牛仔裤",
        price: 99,
        inStock: false,
        imageUrl: "/images/jeans1.jpg",
    },
    {
        id: 4,
        title: "印花 T恤",
        price: 72,
        inStock: true,
        imageUrl: "/images/t-shirt2.jpg",
    },
]);

type SortDirections = "asc" | "desc" | "";

interface SortAndFilter {
    price: SortDirections;
    name: SortDirections;
    inStock: boolean | null;
}

const sortAndFilter: SortAndFilter = reactive({
    price: "",
    name: "",
    inStock: null,
});

const productResult = computed(() => {
    return products.value
        .filter((p) =>
            sortAndFilter.inStock === null
                ? true
                : p.inStock === sortAndFilter.inStock
        )
        .sort((a, b) => {
            if (sortAndFilter.price) {
                return sortAndFilter.price === "asc"
                    ? a.price - b.price
                    : b.price - a.price;
            }
            if (sortAndFilter.name) {
                return sortAndFilter.name === "asc"
                    ? a.title.localeCompare(b.title)
                    : b.title.localeCompare(a.title);
            }
            return 0;
        });
});

const handleSortByPrice = () => {
    if (sortAndFilter.price === "asc") {
        sortAndFilter.price = "desc";
    } else {
        sortAndFilter.price = "asc";
    }
    sortAndFilter.name = "";
};

const handleSortByName = () => {
    if (sortAndFilter.name === "asc") {
        sortAndFilter.name = "desc";
    } else {
        sortAndFilter.name = "asc";
    }
    sortAndFilter.price = "";
};

const handleFilterByStock = (inStock: boolean | null) => {
    sortAndFilter.inStock = inStock;
};
</script>

<template>
    <main>
        <h1>Jack's store</h1>
        <ActionAndFilters
            @sort-by-price="handleSortByPrice"
            @sort-by-name="handleSortByName"
            @filter-by-stock="handleFilterByStock"
        />
        <div class="productList">
            <ProductItem
                v-for="product in productResult"
                :key="product.id"
                v-bind="product"
                class="product"
            >
            </ProductItem>
        </div>
    </main>
</template>

<style scoped>
main {
    display: grid;
    place-items: center;
    gap: 48px;
    width: 100vw;
    padding: 24px;
}

h1 {
    display: flex;
    align-items: center;
    gap: 32px;
}

h1 svg {
    width: 64px;
    height: 64px;
}
.productList {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 48px;
    width: 60%;
}
</style>
